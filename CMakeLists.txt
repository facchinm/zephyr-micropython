# SPDX-FileCopyrightText: Copyright (c) 2024-2026 Henrik Brix Andersen <henrik@brixandersen.dk>
#
# SPDX-License-Identifier: Apache-2.0

include(modules/modules.cmake)

zephyr_include_directories(modules/micropython/include)

# modules/micropython is added to the build via the "micropython" ext module
# entry (see modules/modules.cmake), not from here, to avoid duplicate targets.