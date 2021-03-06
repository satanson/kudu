# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

# See the comment in krb5_realm_override.cc for details on this library's usage.
# The top-level CMakeLists sets a ${KRB5_REALM_OVERRIDE} variable which should
# be linked first into all Kudu binaries.
add_library(krb5_realm_override STATIC krb5_realm_override.cc)
target_link_libraries(krb5_realm_override glog)
if(NOT APPLE)
  target_link_libraries(krb5_realm_override dl)
endif()

add_library(security
  init.cc)
target_link_libraries(security
  gutil
  kudu_util
  krb5)

set(SECURITY_TEST_SRCS
  test/mini_kdc.cc
)

add_library(security-test ${SECURITY_TEST_SRCS})
target_link_libraries(security-test
  gutil
  kudu_test_util
  kudu_util)


# Tests
set(KUDU_TEST_LINK_LIBS
  security
  security-test
  ${KUDU_MIN_TEST_LIBS})

ADD_KUDU_TEST(test/mini_kdc-test)