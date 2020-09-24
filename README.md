Travis-CI: [![Build Status](https://travis-ci.com/nasa/osal.svg)](https://travis-ci.com/nasa/osal)

# Core Flight System : Framework : Operating System Abstraction Layer

This repository contains NASA's Operating System Abstraction Layer (OSAL), which is a framework component of the Core Flight System.

This is a collection of abstractio APIs and associated framework to be located in the `osal` subdirectory of a cFS Mission Tree. The Core Flight System is bundled at <https://github.com/nasa/cFS>, which includes build and execution instructions.

The autogenerated OSAL user's guide can be viewed at <https://github.com/nasa/cFS/blob/gh-pages/OSAL_Users_Guide.pdf>.

## Version History

### Development Build: 5.1.0-rc1+dev44
- Removes OS_Tick2Micros and internalize OS_Milli2Ticks.
- Adds ut_assert address equal macro.
- See <https://github.com/nasa/osal/pull/607>

### Development Build: 5.1.0-rc1+dev38
- Sets Revision to 99 for development builds
- See <https://github.com/nasa/osal/pull/600>


### Development Build: 5.1.0-rc1+dev34

- Move this existing function into the public API, as it is performs more verification than the OS_ConvertToArrayIndex
function.
- The C library type is signed, and this makes the result check work as intended.
- See <https://github.com/nasa/osal/pull/596>

### Development Build: 5.1.0-rc1+dev16

 - In the next major OSAL release, this code will be no longer supported at all. It should be removed early in the cycle to avoid needing to maintain this compatibility code. This code was already conditional on the OSAL_OMIT_DEPRECATED flag and as such the CCB has already tested/verified running the code in this configuration as part of CI scripts. After this change, the build should be equivalent to the result of building with OMIT_DEPRECATED=true.
- See <https://github.com/nasa/osal/pull/582>

### Development Build: 5.1.0-rc1+dev12

- Removes internal functions that are no longer used or defined but whose prototypes and stubs were still present in OS_ObjectIdMap
- Removes repetitive clearing of the global ID and unlocking global table and replaces these with common implementation in the idmap source file. This moves deleting tables to be similar to creating tables and provides
a common location for additional table-deletion-related logic.
- Propagates return code from OS_TaskRegister_Impl(). If this routine fails then return the error to the caller, which also prevents the task from starting.
- See <https://github.com/nasa/osal/pull/576>

### Development Build: 5.1.0-rc1+dev5

- Adds OSAL network APIs missing functional tests as well as tests for OS_TimedRead and OS_TimedWrite
- Allows separate, dynamic registration of test setup and teardown routines which are executed before and after the normal test routine, which can create and delete any global/common test prerequisites.
- Adds FileSysAddFixedMap missing functional API test
- See <https://github.com/nasa/osal/pull/563>


### Development Build: 5.0.0+dev247

- `OS_SocketOpen()` sets `sock_id` and returns a status when successful.
- Changed timer-test to be able to use OS_MAX_TIMERS value on top of the hard-coded NUMBER_OF_TIMERS value. This will allow the test to be functional even if the OS_MAX_TIMERS value is reconfigured.
- Ensures that
  - All stub routines register their arguments in the context, so that the values will be available to hook functions.
  - The argument names used in stubs match the name in the prototype/documentation so the value can be retrieved by name.
- Adds back rounding up to PTHREAD_STACK_MIN and also adds rounding up to a system page size. Keeps check for zero stack at the shared level; attempts to create a task with zero stack will fail. Allows internal helper threads to be created with a default minimum stack size.
- Avoids a possible truncation in snprintf call. No buffer size/truncation warning when building with optimization enabled.
- Added new macros to `osapi-version` to report baseline and build number
- The coverage binaries are now correctly installed for CPU1 and CPU2 as opposed to installed twice to CPU2 but not at all for CPU1.
- Fixes a typo in ut_assert README and clarifies stub documentation.
- See <https://github.com/nasa/osal/pull/529>

### Development Build: 5.0.21

- Command line options in Linux are no longer ignored/dropped.
- No impact to current unit testing which runs UT assert as a standalone app. Add a position independent code (PIC) variant of the ut_assert library, which can be dynamically loaded into other applications rather than running as a standalone OSAL application. This enables loading
UT assert as a CFE library.
- Unit tests pass on RTEMS.
- Resolve inconsistency in how the stack size is treated across different OS implemntations. With this change the user-requested size is passed through to the underlying OS without an enforced minimum. An additional sanity check is added at the shared layer to ensure that the stack size is never passed as 0.
- Update Licenses for Apache 2.0
- See <https://github.com/nasa/osal/pull/521>

### Development Build: 5.0.20
-  Add "non-zero" to the out variable description for OS_Create (and related) API's.
- Increases the buffer for context info from 128 to 256 bytes and the total report buffer to 320 bytes.
- Add stub functions for `OS_TaskFindIdBySystemData()`, `OS_FileSysAddFixedMap()`, `OS_TimedRead()`, `OS_TimedWrite()`, and `OS_FileSysAddFixedMap()`
- Added the following wrappers macros around `UtAssert_True` for commonly-used asserts:
  - `UtAssert_INT32_EQ` - check equality as 32 bit signed int
  - `UtAssert_UINT32_EQ` - check equality as 32 bit unsigned int
  - `UtAssert_NOT_NULL` - check pointer not null
  - `UtAssert_NULL` - check pointer is null
  - `UtAssert_NONZERO` - check integer is nonzero
  - `UtAssert_ZERO` - check integer is zero
  - `UtAssert_STUB_COUNT` - check stub count
-  Using `unsigned long` instead of `uintmax_t` to fix support for VxWorks

- See <https://github.com/nasa/osal/pull/511> and <https://github.com/nasa/osal/pull/513>

### Development Build: 5.0.19

- Rename BSPs that can be used on multiple platforms.
`mcp750-vxworks` becomes `generic-vxworks`
`pc-linux` becomes `generic-linux`
- New features only, does not change existing behavior.
UT Hook functions now have the capability to get argument values by name, which is more future proof than assuming a numeric index.
- Add functional test for `OS_TimerAdd`
- Added functional tests for `OS_TimeBase Api` on `OS_TimeBaseCreate`, `OS_TimeBaseSet`, `OS_TimeBaseDelete`, `OS_TimeBaseGetIdByName`, `OS_TimeBaseGetInfo`, `OS_TimeBaseGetFreeRun`
- See <https://github.com/nasa/osal/pull/487> for details


### Development Build: 5.0.18

- Add functional tests for `OS_IdentifyObject`, `OS_ConvertToArrayIndex` and `OS_ForEachObject` functions.
- Fix doxygen warnings
- Unit test cases which use `OS_statfs` and run on an `RTEMS IMFS` volume will be skipped and categorized as "NA" due to `OS_ERR_NOT_IMPLEMENTED` response, rather than a failure.
- The device_name field was using the wrong length, it should be of `OS_FS_DEV_NAME_LEN` Also correct another length check on the local path name.
- For RTEMS, will not shutdown the kernel if test abort occurs.
- Unit tests work on RTEMS without BSP preallocating ramdisks
- If `OSAL_EXT_SOURCE_DIR` cache variable is set, this location will be checked first for a BSP/OS implementation layer.
- Implement `OS_GetResourceName()` and `OS_ForEachObjectOfType()`, which are new functions that allow for additional query capabilities. No impact to current behavior as the FSW does not currently use any of these new APIs.
- A functional test enhancement to `bin-sem-test` which replicates the specific conditions for the observed bug to occur. Deletes the task calling `OS_BinSemTake()` and then attempts to use the semaphore after this.
- Employ a `pthread` "cleanup handler" to handle the situation where a task is canceled during the `pthread_cond_wait()` call. This ensures that the `mutex` is unlocked as part of the cleanup, so other tasks may continue using the semaphore.    
- Change all initial `mutex` locking to be a finite "timed" wait rather than an infinite wait. In all cases, the condition variable is only held for brief periods of time and should be readily available. If a task blocks for a long time, this considers the mutex "broken" and aborts, thereby avoiding deadlock. This is a "contingency" fix in that if an exception or signal or other unknown/unhandled async event occurs that leaves the mutex permanently locked.
- Adds the mutex to protect the timer callback `timecb` resource table.
- See <https://github.com/nasa/osal/pull/482>

### Development Build: 5.0.17

-  `OS_QueueCreate()` will return an error code if the depth parameter is larger than the configured `OS_MAX_QUEUE_DEPTH`.
- See <https://github.com/nasa/osal/pull/477>

### Development Build: 5.0.16

- Resized buffers and added explicit termination to string copies. No warnings on GCC9 with strict settings and optimization enabled.
- New API to reverse lookup an OS-provided thread/task identifier back to an OSAL ID. Any use of existing OStask_id field within the task property structure is now deprecated.
- See <https://github.com/nasa/osal/pull/458>

### Development Build: 5.0.15

- Changes the build system.
- No more user-maintained osconfig.h file, this is now replaced by a cmake configuration file.
- Breaks up low-level implementation into small, separate subsystem units, with a separate header file for each one.
- See <https://github.com/nasa/osal/pull/444>

### Development Build: 5.0.14

- Adds library build, functional, and coverage test to CI
- Deprecates `OS_FS_SUCCESS, OS_FS_ERROR , OS_FS_ERR_INVALID_POINTER, OS_FS_ERR_NO_FREE_FDS , OS_FS_ERR_INVALID_FD, and OS_FS_UNIMPLEMENTED` from from `osapi-os-filesys.h`
- Individual directory names now limited to OS_MAX_FILE_NAME
- Fix tautology, local_idx1 is now compared with local_idx2
- Module files are generated when the `osal_loader_UT` test is built and run
- Consistent osal-core-test execution status
- See <https://github.com/nasa/osal/pull/440> for more details

### Development Build: 5.0.13

- Added coverage test to `OS_TimerCreate` for `OS_ERR_NAME_TOO_LONG`.
- Externalize enum for `SelectSingle`, ensures that pointers passed to `SelectFd...()` APIs are not null, ensures that pointer to `SelectSingle` is not null.
- Command to run in shell and output to fill will fail with default (not implemented) setting.
- Builds successfully using the inferred OS when only `OSAL_SYSTEM_BSPTYPE` is set. Generates a warning when `OSAL_SYSTEM_BSPTYPE` and `OSAL_SYSTEM_OSTYPE` are both set but are mismatched.
- See <https://github.com/nasa/osal/pull/433> for more details

### Development Build: 5.0.12

- Use the target_include_directories and target_compile_definitions functions from CMake to manage the build flags per target.
- Build implementation components using a separate CMakeLists.txt file rather than aux_source_directory.
- Provide sufficient framework for combining the OSAL BSP, UT BSP, and the CFE PSP and eliminating the duplication/overlap between these items.
- Minor updates (see <https://github.com/nasa/osal/pull/417>)

### Development Build: 5.0.11

- The more descriptive return value OS_ERR_NAME_NOT_FOUND (instead of OS_FS_ERROR) will now be returned from the following functions (): OS_rmfs, OS_mount, OS_unmount, OS_FS_GetPhysDriveName
- Wraps OS_ShMem* prototype and unit test wrapper additions in OSAL_OMIT_DEPRECATED
- Minor updates (see <https://github.com/nasa/osal/pull/408>)

### Development Build: 5.0.10

- Minor updates (see <https://github.com/nasa/osal/pull/401>)

  - 5.0.9: DEVELOPMENT

- Documentation updates (see <https://github.com/nasa/osal/pull/375>)

### Development Build: 5.0.8

- Minor updates (see <https://github.com/nasa/osal/pull/369>)

### Development Build: 5.0.7

- Fixes memset bug
- Minor updates (see <https://github.com/nasa/osal/pull/361>)

### Development Build: 5.0.6

- Minor updates (see <https://github.com/nasa/osal/pull/355>)

### Development Build: 5.0.5

- Fixed osal_timer_UT test failure case
- Minor updates (see <https://github.com/nasa/osal/pull/350>)

### Development Build: 5.0.4

- Minor updates (see <https://github.com/nasa/osal/pull/334>)

### Development Build: 5.0.3

- Minor updates (see <https://github.com/nasa/osal/pull/292>)

### Development Build: 5.0.2

- Bug fixes and minor updates (see <https://github.com/nasa/osal/pull/281>)

### Development Build: 5.0.1

- Minor updates (see <https://github.com/nasa/osal/pull/264>)

### **_OFFICIAL RELEASE: 5.0.0 - Aquila_**

- Changes are detailed in [cFS repo](https://github.com/nasa/cFS) release 6.7.0 documentation
- Released under the Apache 2.0 license

### **_OFFICIAL RELEASE: 4.2.1a_**

- Released under the [NOSA license](https://github.com/nasa/osal/blob/osal-4.2.1a/LICENSE)
- See [version description document](https://github.com/nasa/osal/blob/osal-4.2.1a/OSAL%204.2.1.0%20Version%20Description%20Document.pdf)
- This is a point release from an internal repository

# Quick Start:

Typically OSAL is built and tested as part of cFS as detailed in: [cFS repo](https://github.com/nasa/cFS)

OSAL library build pc-linux example (from the base osal directory):
```
mkdir build_osal
cd build_osal
cmake -DOSAL_SYSTEM_BSPTYPE=generic-linux ..
make
```

OSAL permissive build with tests example (see also [CI](https://github.com/nasa/osal/blob/master/.travis.yml))
```
mkdir build_osal_test
cd build_osal_test
cmake -DENABLE_UNIT_TESTS=true -DOSAL_SYSTEM_BSPTYPE=generic-linux -DOSAL_CONFIG_DEBUG_PERMISSIVE_MODE=TRUE ..
make
make test
```

See the [Configuration Guide](https://github.com/nasa/osal/blob/master/doc/OSAL-Configuration-guide.pdf) for more information.

See also the autogenerated user's guide: <https://github.com/nasa/cFS/blob/gh-pages/OSAL_Users_Guide.pdf>

## Known issues

See all open issues and closed to milestones later than this version.

## Getting Help

For best results, submit issues:questions or issues:help wanted requests at <https://github.com/nasa/cFS>.

Official cFS page: <http://cfs.gsfc.nasa.gov>
