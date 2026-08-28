# GFX.Assets

`GFX.Assets` sépare les données d’assets portables des formats qui les
alimentent. Il ne possède ni entités de scène, ni objets de rendu, ni ressources
GPU, ni générateurs de géométrie 3D.

[Read this documentation in English.](../EN/README.md)

## Installer le package

```text
silex install GFX.Assets
```

GFX.Assets demande Silex 0.39.0 ou une version plus récente.

## Choisir un domaine public

- `GFX.Assets.Image` définit `Image`, `Image.Region`, `Image.Handle` et
  `Image.Store`.
- `GFX.Assets.Model` définit `Model` et ses enregistrements portables,
  notamment `Model.Vertex`, `Model.Geometry`, `Model.Material` et `Model.Node`.
- `GFX.Assets.SpriteSheet` définit `SpriteSheet`, `SpriteSheet.Clip`,
  `SpriteSheet.Handle` et `SpriteSheet.Store`.
- `GFX.Assets.PNG` et `GFX.Assets.GLTF` traduisent des formats externes vers et
  depuis ces valeurs portables.

## Stocker des images

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

`Image.Store` possède les identités et révisions utilisées à l’exécution.
`Image` reste une valeur RGBA8 simple qui ignore comment elle a été décodée.

`Plugins.Assets` installe les stores d’images et de sprite sheets dans une
`Application`. Les mêmes stores sont disponibles sous `Resources.Images` et
`Resources.SpriteSheets` dans les catalogues ouverts de GFX.

## Décrire un modèle portable

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

Ces valeurs décrivent un contenu chargé. La création des entités de scène et
le transfert vers le GPU appartiennent aux packages consommateurs.

## Lire et écrire les formats

`PNG.load` et `PNG.try_load` lisent des fichiers. `PNG.encode`,
`PNG.try_encode`, `PNG.decode` et `PNG.try_decode` travaillent sur des octets.

`GLTF.load`, `GLTF.save`, `GLTF.encode` et `GLTF.decode` forment l’API directe ;
leurs variantes `try_` retournent des erreurs structurées.

PNG et GLTF sont des adaptateurs isolés et transitoires de ce dépôt. Les
domaines portables `Image` et `Model` n’en dépendent pas. Les codecs pourront
donc devenir des packages autonomes sans déplacer ni renommer les types
d’assets ou leurs stores.
