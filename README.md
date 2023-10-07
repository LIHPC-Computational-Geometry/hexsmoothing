# hexsmoothing

Hexahedral mesh smoothing

## Build library

```bash
git clone https://github.com/LIHPC-Computational-Geometry/hexsmoothing
cd hexsmoothing
mkdir build_Release
cd build_Release
cmake .. -DCMAKE_BUILD_TYPE=Release
make
```

## Build library and executables

```bash
git clone https://github.com/LIHPC-Computational-Geometry/hexsmoothing
cd hexsmoothing
mkdir build_Release
cd build_Release
cmake .. -DCMAKE_BUILD_TYPE=Release -DHEXSMOOTHING_LIB_ONLY=OFF
make
```

## License

If you use the library only, the code is distributed under [GPLv3](LICENSE-GPL).

If you use the executables, the code is distributed under [AGPLv3](LICENSE-AGPL).