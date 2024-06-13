# elheif

A wasm heic image encoder and decoder library compiled from [libheif](https://github.com/strukturag/libheif), [libde265](https://github.com/strukturag/libde265) and kvazaar. The library is just a wrapper of `libheif` and compiled to WASM using Emscripten

## API

```ts
interface DecodeImageResult {
  err: string;
  data: Array<{
    width: number;
    height: number;
    /** RGBA8888 bitmap */
    data: Uint8Array;
  }>;
}

interface EncodeImageResult {
  err: string;
  data: Uint8Array;
}

/** Should be called and wait till promise fulfilled when using other APIs */
export function ensureInitialized(): Promise<void>;

/** Convert heic image to RGBA bitmaps */
export function jsDecodeImage(buf: Uint8Array): DecodeImageResult;

/** Convert RGBA bitmap to heic image */
export function jsEncodeImage(
  buf: Uint8Array,
  width: number,
  height: number
): EncodeImageResult;
```

## Usage

```ts
it("decode", async () => {
  await ensureInitialized();

  const buf = loadHeicImage();
  const res = jsDecodeImage(buf);

  expect(res.err).eq("");
  expect(res.data.length).eq(1);
  expect(res.data[0].width).eq(10);
  expect(res.data[0].height).eq(10);
});

it("encode", async () => {
  await ensureInitialized();

  const buf = loadHeicImage();
  const bitmap = jsDecodeImage(buf).data[0];
  const encoded = jsEncodeImage(bitmap.data, bitmap.width, bitmap.height);

  expect(encoded.err).eq("");
  expect(encoded.data.length).eq(1288);
});
```

## License

MIT license. Also note the license for `libheif`, `libde265` and `kvazaar`.
