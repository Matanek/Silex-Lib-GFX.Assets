# GFX.Assets

The package separates portable asset data from the formats that populate it:

- `GFX.Assets.Image` defines `Image`, `Image.Region`, `Image.Handle`, and
  `Image.Store`.
- `GFX.Assets.Model` defines `Model` and its nested portable records such as
  `Model.Vertex`, `Model.Geometry`, `Model.Material`, and `Model.Node`.
- `GFX.Assets.SpriteSheet` defines `SpriteSheet`, `SpriteSheet.Clip`,
  `SpriteSheet.Handle`, and `SpriteSheet.Store`.
- `GFX.Assets.PNG` and `GFX.Assets.GLTF` translate external formats to and
  from those portable values.

## Images and stores

```silex
use GFX.Assets.Image
use GFX.Color

var images = Image.Store()
let handle = images.add(Image.solid(Color.white()))
let snapshot = images.snapshot(handle)
```

`Image.Store` owns runtime identities and revisions. `Image` remains a plain
RGBA8 value and does not know how it was decoded.

`Plugins.Assets` installs the image and sprite-sheet stores into an
`Application`; the same stores are available as `Resources.Images` and
`Resources.SpriteSheets`.

## Portable models

```silex
use GFX.Assets.Model
use STD.Math

let vertices:Model.Vertex[] = [
    Model.Vertex(position:Math.Vec3(-0.5, -0.5, 0.0)),
    Model.Vertex(position:Math.Vec3(0.5, -0.5, 0.0)),
    Model.Vertex(position:Math.Vec3(0.0, 0.5, 0.0))
]
let model = Model.from_geometry(
    Model.Geometry(vertices, [0, 1, 2]),
    Model.Material(),
    "Triangle"
)
```

These values describe loaded content. Scene nodes and GPU uploads belong to
their consuming packages.

## Format adapters

`PNG.load` and `PNG.try_load` read files. `PNG.encode`, `PNG.try_encode`,
`PNG.decode`, and `PNG.try_decode` operate on bytes.

`GLTF.load`, `GLTF.save`, `GLTF.encode`, and `GLTF.decode` provide the ordinary
API. Their `try_` variants return structured failures.

PNG and GLTF are transitional members of this repository. They depend only on
the portable asset domains and may later move to standalone format packages;
that future move must not relocate or rename `Image`, `Model`, or their stores.
