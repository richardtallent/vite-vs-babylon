While BabylonJS 4.2.0 has been stable for some time, something about Vite's newer versions (> b24 or somewhere in that area?) seem to make it impossible to work with BabylonJS's weird implementation of ES Modules.

This is way above my head, the errors I'm getting are inscrutible, but I suspect something is going on with Vite making poor choices about deeper dependencies, and/or BabylonJS not communicating its dependencies properly.
