---
title: '리액트에서 이미지 마스킹하기'
date: 2022-05-30T18:26:25+09:00
category: ['POSTS']
tags: ['Javascript', 'React']
---

```tsx
<Button variant="contained" component="label">
  아이콘 업로드
  <input
    accept="image/*"
    multiple
    type="file"
    hidden
    onChange={(e: ChangeEvent<HTMLInputElement>): void => handleUploadIcon(e)}
  />
</Button>
```

먼저, 업로드 버튼을 만들고, 모든이미지를 허용하고, file타입으로 input태그를 만든다.

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
        //   //! 아이콘 해쉬 변환
        const hash = CryptoJS.SHA256(base64Img).toString()
        const file = dataURLtoFile(base64Img, 'MaskedImage.png')
        dispatch(setInfo(base64Img, 'base64Img'))
        dispatch(setInfo(hash, '아이콘 추가'))
        setFile(file)
      }
    }
  })
}
```

makeImage함수로 이미지를 masking 그리고, dataURLtoFile 함수로 파일객체로 만들어준다.

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

border가 바뀔때마다, 그리고 img가 바뀔때마다, masking 함수를 실행한다.

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

  // blob 타입으로 리턴
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
// * 색칠.
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
// * 이미지로드.
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
