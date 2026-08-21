# GFX.Assets

`GFX.Assets` owns portable image, model, and sprite-sheet data together with
their runtime stores. It deliberately does not own scene entities, render
objects, GPU resources, or 3D geometry generators.

```text
silex install GFX.Assets
```

```silex
use GFX.Assets.Image
use GFX.Color

var images = Image.Store()
let white = images.add(Image.solid(Color.white()))
assert(images.contains(white))
```

The package contributes `Plugins.Assets`, `Resources.Images`, and
`Resources.SpriteSheets` to GFX's open catalogs.

PNG and glTF/GLB reading and writing remain available through
`GFX.Assets.PNG` and `GFX.Assets.GLTF` for this first extraction milestone.
They are isolated adapters: the portable `Image` and `Model` domains do not
depend on either format, so the codecs can later become autonomous packages
without changing the asset types.

See [Docs/README.md](Docs/README.md) for the public domains and codec APIs.
