#
# Copyright (C) 2022  Autodesk, Inc. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0
#

INCLUDE(cxx_defaults)
INCLUDE(rv_lex)

SET(_target
    "WFObj"
)
SET(_yy_prefix
    "wfobj"
)

LEX_IT(
  YY_PREFIX
  ${_yy_prefix}
  INPUT_FILE
  ${CMAKE_CURRENT_SOURCE_DIR}/Lexer.l
  OUTPUT_DIR
  ${CMAKE_CURRENT_SOURCE_DIR}/${_target}/generated
  OUTPUT_FILE
  "FlexLexer.cpp"
)

YACC_IT(
  YY_PREFIX
  ${_yy_prefix}
  INPUT_FILE
  ${CMAKE_CURRENT_SOURCE_DIR}/Grammar.y
  OUTPUT_DIR
  ${CMAKE_CURRENT_SOURCE_DIR}/${_target}/generated
  OUTPUT_FILE
  Grammar.cpp
)

SET(_sources
    Reader.cpp
    # Generated from the above 'lex_it' and 'yacc_it' CMake functions
    WFObj/generated/FlexLexer.cpp WFObj/generated/Grammar.cpp
)

ADD_LIBRARY(
  ${_target} STATIC
  ${_sources}
)

TARGET_INCLUDE_DIRECTORIES(
  ${_target}
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
)

TARGET_COMPILE_OPTIONS(
  ${_target}
  PRIVATE "-DWFOBJ_FLEX_MINOR_VERSION=${RV_FLEX_MINOR_VERSION}"
)

RV_STAGE(TYPE "LIBRARY" TARGET ${_target})
