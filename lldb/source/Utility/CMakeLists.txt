set(LLDB_SYSTEM_LIBS)

# Windows-only libraries
if ( CMAKE_SYSTEM_NAME MATCHES "Windows" )
  list(APPEND LLDB_SYSTEM_LIBS
    ws2_32
    rpcrt4
    )
endif ()

if (NOT LLDB_DISABLE_LIBEDIT)
  list(APPEND LLDB_SYSTEM_LIBS ${libedit_LIBRARIES})
endif()
if (NOT LLDB_DISABLE_CURSES)
  list(APPEND LLDB_SYSTEM_LIBS ${CURSES_LIBRARIES})
  if(LLVM_ENABLE_TERMINFO AND HAVE_TERMINFO)
    list(APPEND LLDB_SYSTEM_LIBS ${TERMINFO_LIBS})
  endif()
endif()

if (NOT HAVE_CXX_ATOMICS64_WITHOUT_LIB )
    list(APPEND LLDB_SYSTEM_LIBS atomic)
endif()

if(Backtrace_FOUND)
  list(APPEND LLDB_SYSTEM_LIBS ${Backtrace_LIBRARY})
endif()

if (NOT LLDB_DISABLE_PYTHON AND NOT LLVM_BUILD_STATIC)
  list(APPEND LLDB_SYSTEM_LIBS ${PYTHON_LIBRARIES})
endif()

list(APPEND LLDB_SYSTEM_LIBS ${system_libs})

if (LLVM_BUILD_STATIC)
  if (NOT LLDB_DISABLE_PYTHON)
    list(APPEND LLDB_SYSTEM_LIBS python2.7 util)
  endif()
  if (NOT LLDB_DISABLE_CURSES)
    list(APPEND LLDB_SYSTEM_LIBS gpm)
  endif()
endif()

add_lldb_library(lldbUtility
  ArchSpec.cpp
  Args.cpp
  Baton.cpp
  Broadcaster.cpp
  Connection.cpp
  ConstString.cpp
  CompletionRequest.cpp
  DataBufferHeap.cpp
  DataBufferLLVM.cpp
  DataEncoder.cpp
  DataExtractor.cpp
  Environment.cpp
  FileCollector.cpp
  Event.cpp
  FileSpec.cpp
  IOObject.cpp
  JSON.cpp
  LLDBAssert.cpp
  Listener.cpp
  Log.cpp
  Logging.cpp
  NameMatches.cpp
  ProcessInfo.cpp
  RegisterValue.cpp
  RegularExpression.cpp
  Reproducer.cpp
  ReproducerInstrumentation.cpp
  Scalar.cpp
  SelectHelper.cpp
  SharingPtr.cpp
  State.cpp
  Status.cpp
  Stream.cpp
  StreamCallback.cpp
  StreamGDBRemote.cpp
  StreamString.cpp
  StringExtractor.cpp
  StringExtractorGDBRemote.cpp
  StringLexer.cpp
  StringList.cpp
  StructuredData.cpp
  TildeExpressionResolver.cpp
  Timer.cpp
  UserID.cpp
  UserIDResolver.cpp
  UriParser.cpp
  UUID.cpp
  VASprintf.cpp
  VMRange.cpp

  LINK_LIBS
    ${LLDB_SYSTEM_LIBS}
    # lldbUtility does not depend on other LLDB libraries

  LINK_COMPONENTS
    BinaryFormat
    Support
  )
