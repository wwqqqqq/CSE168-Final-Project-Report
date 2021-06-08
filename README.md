# CSE 168 Final Project

## Project Proposal
1. Add refractions to GGX model
   * References: 
     * [Microfacet Models for Refraction through Rough Surfaces](https://www.cs.cornell.edu/~srm/publications/EGSR07-btdf.pdf)
     * [Reflections and Refractions in Ray Tracing](https://graphics.stanford.edu/courses/cs148-10-summer/docs/2006--degreve--reflection_refraction.pdf)
2. Add support for `.obj` files
3. Texture mapping
4. Photon mapping
   * References: 
     * [A Practical Guide to Global Illumination using Photon Maps](https://graphics.stanford.edu/courses/cs348b-00/course8.pdf)
     * [Photon Mapping (Global Illumination)](http://www.cs.cmu.edu/afs/cs/academic/class/15462-s15/www/project/p4photon.pdf)

## Milestone
1. implemented refractions of transparent objects in GGX BRDF importance sampling with some bugs on next event estimation.

[Scene1](./scenes/cornell-refraction.test) | [Scene2](./scenes/cornell-refraction2.test)
--- | ---
![img1](./images/cornell-refraction.png) | ![img2](./images/cornell-refraction2.png)

2. Load `.obj` files into the scene.

|[Scene](./scenes/obj-loader.test)|
|---|
|![img](./images/obj-loader-mis.png)|

3. Photon mapping
  
|[Scene](./scenes/cornell-photon.test)|
|---|
|![img](./images/cornell-photon2.png)|

4. Texture mapping

|[Path tracing (MIS)](./scenes/texture.test)|[Photon mapping](./scenes/texture-photon.test) | [Path tracing (NEE off)](./scenes/texture-nee-off.test)
|---| --- | --- |
| 2048 spp, MIS |512 spp, 4M photons, 0.04 radius| 4096 spp, NEE off |
|![img](./images/texture.png)|![img](./images/texture-photon.png)|![img](./images/texture-no-nee.png)

## Documentation

Transparent object rendered with GGX refraction model:
```
brdf ggx-refraction
ior [float] # eta0 = front face index of refraction
            # eta1 = back face index of refraction
            # ior = eta0 / eta1, default = 1.0
```
Load `.obj` file into the scene:
```
obj [filename.obj] # use the same commands to specify transforms and material.
                   # the program will automatically scale and center the model 
                   # so that the model is within the cube [-1,1]x[-1,1]x[-1,1].
```

Photon mapping:
```
integrator photonmapping
ppl [int] # number of photons per light
          # default is 100,000
photonSearchRadius [float] # photon search radius
                           # default = 0.1
```

Load textures:
*In `.test` files*
```
texture [textureFile] # texture file (hdr, ppm format)
                      # only support one texture for a scene
texCoord [x] [y]      # define a vertex (x,y) 
                      # in the texture coordinate
texTri [v1] [v2] [v3] [t1] [t2] [t3] # define triangles with textures
```

*In `.obj` files*
```
vt x y                          # define a vertex (x,y) in texture coordinates
f v1/t1/vn1 v2/t2/vn2 v3/t3/vn3 # define triangle faces with vertices, 
                                # texture coordinates, and normals
```