---
title: 'Image Masking in React'
author: 'Hun Im'
date: 2022-05-30T18:26:25+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
og_image: "/images/gamer.png" 
keywords: ['Javascript', 'React']
---

```tsx
<Button variant="contained" component="label">
  Upload Icon
  <input
    accept="image/*"
    multiple
    type="file"
    hidden
    onChange={(e: ChangeEvent<HTMLInputElement>): void => handleUploadIcon(e)}
  />
</Button>
```

First, we create an upload button that accepts all images and creates an input tag of type file.

```tsx
const handleUploadIcon = (e: ChangeEvent<HTMLInputElement>) => {
  setImgFile(e.target.files[0])

  const imagePromise = e.target.files[0].arrayBuffer().then((buf) => {
    return makeImage(buf, figure, borderColor)
  })

  imagePromise.then((resized) => {
    const fileReader = new FileReader()
    fileReader.readAsDataURL(resized)
    fileReader.onload = (e) => {
      const image = new Image()
      image.src = e.target.result as string
      image.onload = () => {
        const base64Img = e.target.result as string
        //   //! Convert icon to hash
        const hash = CryptoJS.SHA256(base64Img).toString()
        const file = dataURLtoFile(base64Img, 'MaskedImage.png')
        dispatch(setInfo(base64Img, 'base64Img'))
        dispatch(setInfo(hash, 'Icon Added'))
        setFile(file)
      }
    }
  })
}
```

We use the `makeImage` function to mask the image and the `dataURLtoFile` function to create a file object.

```tsx
useEffect(() => {
  if (imgFile) {
    const imagePromise = imgFile.arrayBuffer().then((buf: ArrayBuffer) => {
      return makeImage(buf, figure, borderColor)
    })

    imagePromise.then((resized: Blob) => {
      const fileReader = new FileReader()
      fileReader.readAsDataURL(resized)
      fileReader.onload = (e) => {
        const image = new Image()
        image.src = e.target.result as string
        image.onload = () => {
          const base64Img = e.target.result as string
          const hash = CryptoJS.SHA256(base64Img).toString()
          const file = dataURLtoFile(base64Img, 'MaskedImage.png')
          dispatch(setInfo(base64Img, 'base64Img'))
          dispatch(setInfo(hash, '아이콘 추가'))
          setFile(file)
        }
      }
    })
  }
}, [borderImgState, borderColor])

useEffect(() => {
  if (icon && file) {
    dispatch(setInfo(file, 'uploadFile'))
    dispatch(setInfo(borderImg, 'borderImgState'))
    dispatch(setInfo(borderColor, 'borderColor'))
  }
}, [icon, file])
```

The masking function is executed every time the border or image changes.

```ts
/* eslint-disable func-names */
/* eslint-disable no-bitwise */
/* eslint-disable no-plusplus */
import { Image as ImageJS } from 'image-js'
import { lineURL, pngURL } from '../../components/TokenDeploy/MakeSymbol'

const BASE_SIZE = 640

async function makeImage(
  imgBytes: ArrayBuffer,
  templateNum: number,
  borderColor: string
) {
  // 1. load image from bytes
  const originalImg = await ImageJS.load(imgBytes)
  const emptyArr = new Array(640 * 640 * 4).fill(0)
  const bg = new ImageJS(BASE_SIZE, BASE_SIZE, emptyArr, { alpha: 1 })

  console.log(
    'original originalImg info',
    originalImg.width,
    originalImg.height
  )

  // 2. resize image
  let xOffset = 0
  let yOffset = 0
  let resizedImg: ImageJS
  if (originalImg.width > originalImg.height) {
    resizedImg = originalImg.resize({ width: BASE_SIZE })
    yOffset = (BASE_SIZE - resizedImg.height) / 2
  } else {
    resizedImg = originalImg.resize({ height: BASE_SIZE })
    xOffset = (BASE_SIZE - resizedImg.width) / 2
  }
  const squaredImg = await mergeImages(bg, xOffset, yOffset, resizedImg)

  // 3. make maskImage
  const maskOriginal = await makeMaskImage(templateNum)
  const mask: ImageJS = maskOriginal
    // @ts-ignore
    .gray()
    .mask({ threshold: 0.1, useAlpha: true })

  // 4. extract by mask
  // @ts-ignore
  const maskedImage = await squaredImg.extract(mask, { position: [0, 0] })

  // 5. draw border image
  const borderImg = await makeBorderImage(templateNum, borderColor)
  const borderAddedImage = await mergeImages(maskedImage, 0, 0, borderImg)

  // Return as blob type
  return borderAddedImage.toBlob()
}

async function makeMaskImage(templateNumber: number): Promise<ImageJS> {
  const borderMaskFile = pngURL[templateNumber]
  const maskUrl = `${borderMaskFile}`

  return new Promise<ImageJS>(function (resolve, reject) {
    const xhr = new XMLHttpRequest()
    xhr.open('get', maskUrl)
    xhr.responseType = 'arraybuffer'
    xhr.onload = function () {
      ImageJS.load(xhr.response).then((mask) => resolve(mask))
    }
    xhr.onerror = function () {
      reject(new Error(`failed by ${this.status}`))
    }
    xhr.send()
  })
}
// * Coloring.
async function makeBorderImage(
  templateNumber: number,
  borderColor: string
): Promise<ImageJS> {
  const rawBorderMask = await loadBorderImage(templateNumber)
  // @ts-ignore
  const mask = rawBorderMask.gray().mask({ threshold: 0.1, useAlpha: true })
  const background = new ImageJS(BASE_SIZE, BASE_SIZE, [], { alpha: 1 })
  // @ts-ignore
  const maskedBorder = background.extract(mask, { position: [0, 0] })
  const border = maskedBorder.paintMasks(mask, { color: borderColor })
  return border
}
// * Load image.
async function loadBorderImage(templateNumber: number): Promise<ImageJS> {
  const borderMaskFile = lineURL[templateNumber]
  const borderUrl = `${borderMaskFile}`

  return new Promise<ImageJS>(function (resolve, reject) {
    const xhr = new XMLHttpRequest()
    xhr.open('get', borderUrl)
    xhr.responseType = 'arraybuffer'
    xhr.onload = function () {
      ImageJS.load(xhr.response).then((mask) => resolve(mask))
    }
    xhr.onerror = function () {
      reject(new Error(`failed by ${this.status}`))
    }
    xhr.send()
  })
}

async function mergeImages(
  out: ImageJS,
  x: number,
  y: number,
  toInsert: ImageJS
) {
  const maxY = Math.min(out.height, y + toInsert.height)
  const maxX = Math.min(out.width, x + toInsert.width)

  if (out.bitDepth === 1) {
    for (let j = y; j < maxY; j++) {
      for (let i = x; i < maxX; i++) {
        const val = toInsert.getBitXY(i - x, j - y)
        if (val) out.setBitXY(i, j)
        else out.clearBitXY(i, j)
      }
    }
  } else {
    for (let j = y; j < maxY; j++) {
      for (let i = x; i < maxX; i++) {
        if (toInsert.getPixelXY(i - x, j - y)[3] > 127) {
          out.setPixelXY(i, j, toInsert.getPixelXY(i - x, j - y))
        }
      }
    }
  }

  return out
}

export default makeImage
```

```ts
const dataURLtoFile = (base64: string, fileName: string) => {
  const arr = base64.split(',')
  const mime = arr[0].match(/:(.*?);/)[1]
  const bstr = window.atob(arr[1])
  let n = bstr.length
  const u8arr = new Uint8Array(n)

  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }

  const file = new File([u8arr], `${fileName}.png`, { type: mime })
  return file
}
```
