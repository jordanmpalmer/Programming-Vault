
## [SDK Documentation](https://developer.x-plane.com/sdk/)
## [Cmake SDK Helper Github](https://github.com/leecbaker/xplane_sdk/tree/master)
## [PPL Github](https://github.com/JDeeth/PPL)
## [PPL Demo Github](https://github.com/JDeeth/PPL-Demo)

https://pewpewthespells.com/blog/managing_xcode.html#buildconf
https://pewpewthespells.com/blog/xcode_build_locations.html


Four main steps in the compilation process:
1. Preprocess. (That is, convert anything starting with “#” into something else.)
2. Compile. (That is, convert C into Assembly.)
3. Assemble. (That is, convert Assembly into machine-language object modules.)
4. Link. (That is, link the various modules in the project together, and to external library modules, in order to resolve all cross-module function calls. Also link to any necessary modules within the operating system, resolve all OS API calls, and create program entry point. If/when all calls are resolved, write the linked machine code to a “*.exe” file.