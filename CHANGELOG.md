# Changelog for hipCUB

Documentation for hipCUB is available at
[https://rocm.docs.amd.com/projects/hipCUB/en/latest/](https://rocm.docs.amd.com/projects/hipCUB/en/latest/).

## (Unreleased) hipCUB-3.1.0 for ROCm 6.1.0

### Changes

* CUB backend references CUB and Thrust version 2.1.0
* Updated the `HIPCUB_HOST_WARP_THREADS` macro definition to match `host_warp_size` changes
  from rocPRIM 3.0
* Implemented `__int128_t` and `__uint128_t` support for `radix_sort`

### Fixes

* Build issues with `rmake.py` on Windows when using Visual Studio 2017 15.8 or later (due to a
  breaking fix with extended aligned storage)

### Additions

* Interface `DeviceMemcpy::Batched` for batched memcpy from rocPRIM and CUB

## hipCUB-3.0.0 for ROCm 6.0.0

### Changes

* Removed `DOWNLOAD_ROCPRIM`
  * You can force rocPRIM to download using `DEPENDENCIES_FORCE_DOWNLOAD`

## hipCUB-2.13.2 for ROCm 5.7.0

### Changes

* CUB backend references CUB and Thrust version 2.0.1.
* Fixed `DeviceSegmentedReduce::ArgMin` and `DeviceSegmentedReduce::ArgMax` by returning the
  segment-relative index instead of the absolute one
* Fixed `DeviceSegmentedReduce::ArgMin` for inputs where the segment minimum is smaller than the
  value returned for empty segments; an equivalent fix is applied to `DeviceSegmentedReduce::ArgMax`

### Known issues

* `debug_synchronous` no longer works on the CUDA platform; use `CUB_DEBUG_SYNC` instead
* `DeviceReduce::Sum` doesn't compile on the CUDA platform for mixed extended-floating-point or
  floating-point InputT and OutputT types
* `DeviceHistogram::HistogramEven` fails on CUDA platform for `[LevelT, SampleIteratorT] = [int, int]`.
* `DeviceHistogram::MultiHistogramEven` fails on CUDA platform for
  `[LevelT, SampleIteratorT] = [int, int/unsigned short/float/double]` and
  `[LevelT, SampleIteratorT] = [float, double]`

## hipCUB-2.13.1 for ROCm 5.5.0

### Additions

* Benchmarks for `BlockShuffle`, `BlockLoad`, and `BlockStore`

### Changes

* The CUB backend references CUB and Thrust version 1.17.2
* Improved benchmark coverage for:
  * `BlockScan` by adding `ExclusiveScan`
  * `BlockRadixSort` by adding `SortBlockedToStriped`
  * `WarpScan` by adding `Broadcast`
* Removed references to, and workarounds for, the deprecated hcc

### Known issues

* `BlockRadixRankMatch` is currently broken for the rocPRIM backend
* `BlockRadixRankMatch` with a warp size that does not divide exactly by the block size is broken for
  the CUB backend

## hipCUB-2.13.0 for ROCm 5.4.0

### Additions

* CMake functionality improves build parallelism for the test suite that splits compilation units by
function or parameters
* New overload for `BlockAdjacentDifference::SubtractLeftPartialTile`, which takes a predecessor item

### Changes

* Improved build parallelism of the test suite by splitting up large compilation units for `DeviceRadixSort`, `DeviceSegmentedRadixSort`, and `DeviceSegmentedSort`
* The CUB backend references CUB and Thrust version 1.17.1

### Known issues

* `BlockRadixRankMatch` is currently broken for the rocPRIM backend
* `BlockRadixRankMatch` with a warp size that does not divide exactly by the block size is broken for
  the CUB backend

## hipCUB-2.12.0 for ROCm 5.3.0

### Additions

* `UniqueByKey` device algorithm
* `SubtractLeft`, `SubtractLeftPartialTile`, `SubtractRight`, and `SubtractRightPartialTile` overload in
  `BlockAdjacentDifference`
  * The old overloads (`FlagHeads`, `FlagTails`, `FlagHeadsAndTails`) are deprecated
* `DeviceAdjacentDifference` algorithm
* Extended benchmark suite of `DeviceHistogram`, `DeviceScan`, `DevicePartition`, `DeviceReduce`,
`DeviceSegmentedReduce`, `DeviceSegmentedRadixSort`, `DeviceRadixSort`, `DeviceSpmv`,
`DeviceMergeSort`, and `DeviceSegmentedSort`

### Changes

* Obsolete type traits defined in `util_type.hpp`; use the standard library equivalents instead
* CUB backend references CUB and Thrust version 1.16.0
* `DeviceRadixSort` `num_items` parameter type is now templated instead of being an int
  * If an integral type with a maximum size of 4 bytes is passed (an int), the former logic applies;
    otherwise, the algorithm uses a larger indexing type that makes it possible to sort input data over
    $2^{32}$ elements

## hipCUB-2.11.1 for ROCm 5.2.0

### Additions

* Packages for tests and benchmark executables on all supported operating systems using CPack

## hipCUB-2.11.0 for ROCm 5.1.0

### Additions

* Device segmented sort
* `WarpMergeSort`, `WarpMask`, and thread sort from CUB 1.15.0 are supported in hipCUB
* Device three-way partition

### Changes

* `device_scan` and `device_segmented_scan`: `inclusive_scan` now uses the `input-type` as
  `accumulator-type`; `exclusive_scan` uses `initial-value-type`.
  * This changes the behavior of:
    * Small-size input types with large-size output types (e.g., short input, int output)
    * Low-res input with high-res output (e.g., float input, double output)
* Block merge sort no longer supports non-power of two block sizes

### Known issues

* Grid unit test hangs on HIP for Windows

## hipCUB-2.10.13 for ROCm 5.0.0

### Fixes

* Added missing includes to `hipcub.hpp`

### Additions

* Bfloat16 support to test cases (`device_reduce` and `device_radix_sort`)
* Device merge sort
* Block merge sort
* API update to CUB 1.14.0

### Changes

* The `SetupNVCC.cmake` automatic target selector selects all of the capabilities for all available cards
  with the NVIDIA backend

## hipCUB-2.10.12 for ROCm 4.5.0

### Additions

* Initial HIP on Windows support

### Changes

* Packaging changed to a development package (named `hipcub-dev` for `.deb` packages and
  `hipcub-devel` for `.rpm` packages). Because hipCUB is a header-only library, there is no runtime
  package. To aid in the transition, the development package sets the `provides` field to `hipcub`, so
  existing packages that are dependent on hipCUB can continue to work. This `provides` feature is
  introduced as a deprecated feature because it will be removed in a future ROCm release.

## hipCUB-2.10.11 for ROCm 4.4.0

### Additions

* gfx1030 support added
* AddressSanitizer build option

### Fixes

* `BlockRadixRank` unit test failure

## hipCUB-2.10.10 for ROCm 4.3.0

### Additions

* `DiscardOutputIterator` to backend header

## hipCUB-2.10.9 for ROCm 4.2.0

### Additions

* Support for `TexObjInputIterator` and `TexRefInputIterator`
* Support for `DevicePartition`

### Changes

* The minimum CMake version required is now 3.10.2
* The CUB backend has been updated to 1.11.0

### Fixes

* Benchmark build
* NVCC build

## hipCUB-2.10.8 for ROCm 4.1.0

### Added

* Support for `DiscardOutputIterator`

## hipCUB-2.10.7 for ROCm 4.0.0

* No changes

## hipCUB-2.10.6 for ROCm 3.10

* No changes

## hipCUB-2.10.5 for ROCm 3.9.0

* No changes

## hipCUB-2.10.4 for ROCm 3.8.0

* No changes

## hipCUB-2.10.3 for ROCm 3.7.0

* No changes

## hipCUB-2.10.2 for ROCm 3.6.0

* No changes

## hipCUB-2.10.1 for ROCm 3.5.0

### Additions

* Improved tests with fixed and random seeds for test data

### Changes

* Switched to hip-clang as default compiler
* CMake searches for rocPRIM locally first and, if not found, downloads it from GitHub

### Deprecations

* HCC build

### Known issues

* The following unit test failures (due to issues in ROCclr runtime) have been observed:
  * `BlockDiscontinuity`
  * `BlockExchange`
  * `BlockHistogram`
  * `BlockRadixSort`
  * `BlockReduce`
  * `BlockScan`
