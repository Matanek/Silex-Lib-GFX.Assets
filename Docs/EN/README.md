# GFX.Assets

`GFX.Assets` separates portable asset data from the formats that populate it.
It does not own scene entities, render objects, GPU resources, or 3D geometry
generators.

[Lire cette documentation en français.](../FR/README.md)

## Install the package

```text
silex install GFX.Assets
```

This development revision requires the toolchain with `try` capture for
`Result` operations and extraction on ordinary calls.

## Choose a public domain

- `GFX.Assets.Image` defines `Image`, `Image.Region`, `Image.Handle`, and
  `Image.Store`.
- `GFX.Assets.Model` defines `Model` and its portable records, including
  `Model.Vertex`, `Model.Geometry`, `Model.Material`, and `Model.Node`.
- `GFX.Assets.SpriteSheet` defines `SpriteSheet`, `SpriteSheet.Clip`,
  `SpriteSheet.Handle`, and `SpriteSheet.Store`.
- `GFX.Assets.PNG` and `GFX.Assets.GLTF` translate external formats to and from
  those portable values.

## Store images

```sx
use GFX.Assets.Image
use GFX.Color

func main() {
    var images = Image.Store()
    let handle = images.add(Image.solid(Color.white()))
    let snapshot = images.snapshot(handle)
    assert(images.contains(handle))
}
```

`Image.Store` owns runtime identities and revisions. `Image` remains a plain
RGBA8 value that does not know how it was decoded.

`Plugins.Assets` installs the image and sprite-sheet stores into an
`Application`. The same stores are available as `Resources.Images` and
`Resources.SpriteSheets` in GFX's open catalogs.

## Describe a portable model

```sx
use GFX.Assets.Model
use STD.Math

func main() {
    let vertices:Model.Vertex[] = [
        Model.Vertex(position:Math.Vec3(-0.5, -0.5, 0.0)),
        Model.Vertex(position:Math.Vec3(0.5, -0.5, 0.0)),
        Model.Vertex(position:Math.Vec3(0.0, 0.5, 0.0)),
    ]
    let model = Model.from_geometry(
        Model.Geometry(vertices, [0, 1, 2]),
        Model.Material(),
        "Triangle",
    )
}
```

These values describe loaded content. Scene entity creation and GPU uploads
belong to consuming packages.

## Read and write formats

`PNG.load` reads a file; `PNG.encode` and `PNG.decode` operate on bytes.
`GLTF.load`, `GLTF.save`, `GLTF.encode`, and `GLTF.decode` use the same model:
each operation has one `Result` declaration. An ordinary call returns the
value or stops the program on failure. Add `try` to keep the result and handle
a structured error.

PNG and GLTF are isolated, transitional adapters in this repository. The
portable `Image` and `Model` domains do not depend on them. The codecs can
therefore become standalone packages without moving or renaming asset types or
their stores.
