cmake_minimum_required(VERSION 3.16)
project(deepin-liferaft CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(QT_MIN_VERSION "6.11.1")
set(KF6_MIN_VERSION "6.28.0")

find_package(Qt6 ${QT_MIN_VERSION} NO_MODULE COMPONENTS Widgets REQUIRED)
find_package(KF6I18n ${KF6_MIN_VERSION})   # 引入 ki18n 支持

add_executable(deepin-liferaft main.cpp resources.qrc)

# 链接必要的库，并启用 HAVE_KF6 宏以使用 i18n()
target_link_libraries(deepin-liferaft PRIVATE Qt6::Widgets KF6::I18n)
target_compile_definitions(deepin-liferaft PRIVATE HAVE_KF6)

# 处理翻译文件（假设 po 目录位于源码根目录）
if(EXISTS ${CMAKE_SOURCE_DIR}/po)
    ki18n_install(po)   # 自动将 po/*.po 编译为 .mo 并安装到 share/locale
endif()

enable_testing()
add_test(NAME self-test COMMAND deepin-liferaft --self-test)

install(TARGETS deepin-liferaft RUNTIME DESTINATION bin)
install(FILES deepin-liferaft.service DESTINATION lib/systemd/user)
install(FILES deepin-liferaft.desktop DESTINATION share/applications)
install(FILES data/icons/deepin-liferaft.svg DESTINATION share/icons/hicolor/scalable/apps)
install(FILES deepin-liferaft.1 DESTINATION share/man/man1)
install(FILES README.md DESTINATION share/doc/deepin-liferaft)