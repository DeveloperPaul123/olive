cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(olive-editor LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(OpenGL REQUIRED)

find_package(Qt5 5.7 REQUIRED
  COMPONENTS
  Core
  Gui
  Multimedia
  OpenGL
  Svg
  LinguistTools
)

find_package(FFMPEG 3.4 REQUIRED
  COMPONENTS
  avutil
  avcodec
  avformat
  avfilter
  swscale
  swresample
)

find_package(frei0r)
if(NOT FREI0R_FOUND)
  add_definitions(-DNOFREI0R)
endif()

find_package(Git)
if(GIT_FOUND)
  execute_process(COMMAND ${GIT_EXECUTABLE} log -1 --format=%h
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
  )
  add_definitions(-DGITHASH="${GIT_HASH}")
endif()

set(olive_SOURCES
  main.cpp
  mainwindow.cpp
  panels/project.cpp
  panels/effectcontrols.cpp
  panels/viewer.cpp
  panels/timeline.cpp
  ui/sourcetable.cpp
  dialogs/aboutdialog.cpp
  ui/timelinewidget.cpp
  project/media.cpp
  project/footage.cpp
  project/sequence.cpp
  project/clip.cpp
  io/config.cpp
  dialogs/newsequencedialog.cpp
  ui/viewerwidget.cpp
  ui/viewercontainer.cpp
  dialogs/exportdialog.cpp
  ui/collapsiblewidget.cpp
  panels/panels.cpp
  io/exportthread.cpp
  ui/timelineheader.cpp
  io/previewgenerator.cpp
  ui/labelslider.cpp
  dialogs/preferencesdialog.cpp
  ui/audiomonitor.cpp
  project/undo.cpp
  ui/scrollarea.cpp
  ui/comboboxex.cpp
  ui/colorbutton.cpp
  dialogs/replaceclipmediadialog.cpp
  ui/fontcombobox.cpp
  ui/checkboxex.cpp
  ui/keyframeview.cpp
  ui/texteditex.cpp
  dialogs/demonotice.cpp
  project/marker.cpp
  dialogs/speeddialog.cpp
  dialogs/mediapropertiesdialog.cpp
  project/projectmodel.cpp
  io/loadthread.cpp
  dialogs/loaddialog.cpp
  debug.cpp
  io/path.cpp
  effects/internal/linearfadetransition.cpp
  effects/internal/transformeffect.cpp
  effects/internal/solideffect.cpp
  effects/internal/texteffect.cpp
  effects/internal/timecodeeffect.cpp
  effects/internal/audionoiseeffect.cpp
  effects/internal/paneffect.cpp
  effects/internal/toneeffect.cpp
  effects/internal/volumeeffect.cpp
  effects/internal/crossdissolvetransition.cpp
  effects/internal/shakeeffect.cpp
  effects/internal/exponentialfadetransition.cpp
  effects/internal/logarithmicfadetransition.cpp
  effects/internal/cornerpineffect.cpp
  io/math.cpp
  io/qpainterwrapper.cpp
  project/effect.cpp
  project/transition.cpp
  project/effectrow.cpp
  project/effectfield.cpp
  effects/internal/cubetransition.cpp
  project/effectgizmo.cpp
  io/clipboard.cpp
  ui/resizablescrollbar.cpp
  ui/sourceiconview.cpp
  project/sourcescommon.cpp
  ui/keyframenavigator.cpp
  panels/grapheditor.cpp
  ui/graphview.cpp
  ui/keyframedrawing.cpp
  ui/clickablelabel.cpp
  project/keyframe.cpp
  ui/rectangleselect.cpp
  dialogs/actionsearch.cpp
  ui/embeddedfilechooser.cpp
  effects/internal/fillleftrighteffect.cpp
  effects/internal/voideffect.cpp
  dialogs/texteditdialog.cpp
  dialogs/debugdialog.cpp
  ui/viewerwindow.cpp
  project/projectfilter.cpp
  effects/internal/frei0reffect.cpp
  project/effectloaders.cpp
  io/crossplatformlib.cpp
  effects/internal/vsthost.cpp
  ui/flowlayout.cpp
  dialogs/proxydialog.cpp
  io/proxygenerator.cpp
  dialogs/advancedvideodialog.cpp
  ui/cursors.cpp
  ui/menuhelper.cpp
  oliveglobal.cpp
  ui/focusfilter.cpp
  project/comboaction.cpp
  ui/mediaiconservice.cpp
  ui/panel.cpp
  effects/internal/dropshadoweffect.cpp
  rendering/renderfunctions.cpp
  rendering/renderthread.cpp
  rendering/cacher.cpp
  rendering/clipqueue.cpp
  rendering/audio.cpp
  dialogs/clippropertiesdialog.cpp
  rendering/framebufferobject.cpp
  ui/updatenotification.cpp
  ui/icons.cpp
)

set(olive_HEADERS
  mainwindow.h
  panels/project.h
  panels/effectcontrols.h
  panels/viewer.h
  panels/timeline.h
  ui/sourcetable.h
  dialogs/aboutdialog.h
  ui/timelinewidget.h
  project/media.h
  project/footage.h
  project/sequence.h
  project/clip.h
  io/config.h
  dialogs/newsequencedialog.h
  ui/viewerwidget.h
  ui/viewercontainer.h
  dialogs/exportdialog.h
  ui/collapsiblewidget.h
  panels/panels.h
  io/exportthread.h
  ui/timelinetools.h
  ui/timelineheader.h
  io/previewgenerator.h
  ui/labelslider.h
  dialogs/preferencesdialog.h
  ui/audiomonitor.h
  project/undo.h
  ui/scrollarea.h
  ui/comboboxex.h
  ui/colorbutton.h
  dialogs/replaceclipmediadialog.h
  ui/fontcombobox.h
  ui/checkboxex.h
  ui/keyframeview.h
  ui/texteditex.h
  dialogs/demonotice.h
  project/marker.h
  project/selection.h
  dialogs/speeddialog.h
  dialogs/mediapropertiesdialog.h
  project/projectmodel.h
  io/loadthread.h
  dialogs/loaddialog.h
  debug.h
  io/path.h
  effects/internal/transformeffect.h
  effects/internal/solideffect.h
  effects/internal/texteffect.h
  effects/internal/timecodeeffect.h
  effects/internal/audionoiseeffect.h
  effects/internal/paneffect.h
  effects/internal/toneeffect.h
  effects/internal/volumeeffect.h
  effects/internal/shakeeffect.h
  effects/internal/linearfadetransition.h
  effects/internal/crossdissolvetransition.h
  effects/internal/exponentialfadetransition.h
  effects/internal/logarithmicfadetransition.h
  effects/internal/cornerpineffect.h
  io/math.h
  io/qpainterwrapper.h
  project/effect.h
  project/transition.h
  project/effectrow.h
  project/effectfield.h
  effects/internal/cubetransition.h
  project/effectgizmo.h
  io/clipboard.h
  ui/resizablescrollbar.h
  ui/sourceiconview.h
  project/sourcescommon.h
  ui/keyframenavigator.h
  panels/grapheditor.h
  ui/graphview.h
  ui/keyframedrawing.h
  ui/clickablelabel.h
  project/keyframe.h
  ui/rectangleselect.h
  dialogs/actionsearch.h
  ui/embeddedfilechooser.h
  effects/internal/fillleftrighteffect.h
  effects/internal/voideffect.h
  dialogs/texteditdialog.h
  dialogs/debugdialog.h
  ui/viewerwindow.h
  project/projectfilter.h
  effects/internal/frei0reffect.h
  project/effectloaders.h
  io/crossplatformlib.h
  effects/internal/vsthost.h
  ui/flowlayout.h
  dialogs/proxydialog.h
  io/proxygenerator.h
  dialogs/advancedvideodialog.h
  ui/cursors.h
  ui/menuhelper.h
  oliveglobal.h
  project/projectelements.h
  ui/focusfilter.h
  project/comboaction.h
  ui/mediaiconservice.h
  ui/panel.h
  effects/internal/dropshadoweffect.h
  rendering/renderfunctions.h
  rendering/renderthread.h
  rendering/clipqueue.h
  rendering/cacher.h
  rendering/audio.h
  dialogs/clippropertiesdialog.h
  rendering/framebufferobject.h
  ui/updatenotification.h
  ui/icons.h
)

set(olive_RESOURCES
  icons/icons.qrc
  effects/internal/internalshaders.qrc
  cursors/cursors.qrc
)

set(olive_EFFECTS
  effects/boxblur.frag
  effects/boxblur.xml
  effects/bulge.frag
  effects/bulge.xml
  effects/chromakey.frag
  effects/chromakey.xml
  effects/chromaticaberration.frag
  effects/chromaticaberration.xml
  effects/colorcorrection.frag
  effects/colorcorrection.xml
  effects/colorsel.frag
  effects/colorsel.xml
  effects/common.frag
  effects/common.vert
  effects/crop.frag
  effects/crop.xml
  effects/crossstitch.frag
  effects/crossstitch.xml
  effects/directionalblur.frag
  effects/directionalblur.xml
  effects/dropshadow.xml.disabled
  effects/emboss.frag
  effects/emboss.xml
  effects/findedges.frag
  effects/findedges.xml.disabled
  effects/fisheye.frag
  effects/fisheye.xml
  effects/flip.frag
  effects/flip.xml
  effects/gaussianblur.frag
  effects/gaussianblur.xml
  effects/huesatbri.frag
  effects/huesatbri.xml
  effects/invert.frag
  effects/invert.xml
  effects/lumakey.frag
  effects/lumakey.xml
  effects/noise.frag
  effects/noise.xml
  effects/pixelate.frag
  effects/pixelate.xml
  effects/posterize.frag
  effects/posterize.xml
  effects/radialblur.frag
  effects/radialblur.xml
  effects/ripple.frag
  effects/ripple.xml
  effects/sphere.frag
  effects/sphere.xml
  effects/swirl.frag
  effects/swirl.xml
  effects/tile.frag
  effects/tile.xml
  effects/toonify.frag
  effects/toonify.xml
  effects/vignette.frag
  effects/vignette.xml
  effects/volumetriclight.frag
  effects/volumetriclight.xml
  effects/wave.frag
  effects/wave.xml
)

qt5_add_translation(QM_FILES
  ts/olive_ar.ts
  ts/olive_bs.ts
  ts/olive_cs.ts
  ts/olive_de.ts
  ts/olive_es.ts
  ts/olive_fr.ts
  ts/olive_it.ts
  ts/olive_ru.ts
  ts/olive_sr.ts
)

set(TARGET_NAME "olive-editor")
if(APPLE)
  set(TARGET_NAME "Olive")
endif()

add_executable(${TARGET_NAME}
  ${olive_SOURCES}
  ${olive_HEADERS}
  ${olive_RESOURCES}
  ${QM_FILES}
)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

target_include_directories(${TARGET_NAME}
  PUBLIC
  ${FFMPEG_avutil_INCLUDE_DIRS}
  ${FFMPEG_avcodec_INCLUDE_DIRS}
  ${FFMPEG_avformat_INCLUDE_DIRS}
  ${FFMPEG_avfilter_INCLUDE_DIRS}
  ${FFMPEG_swscale_INCLUDE_DIRS}
  ${FFMPEG_swresample_INCLUDE_DIRS}
)

target_link_libraries(${TARGET_NAME}
  OpenGL::GL
  Qt5::Core
  Qt5::Gui
  Qt5::Multimedia
  Qt5::OpenGL
  Qt5::Svg
  FFMPEG::avutil
  FFMPEG::avcodec
  FFMPEG::avformat
  FFMPEG::avfilter
  FFMPEG::swscale
  FFMPEG::swresample
  ${CMAKE_DL_LIBS}
)

if(UNIX AND NOT APPLE)
  install(TARGETS ${TARGET_NAME} RUNTIME DESTINATION bin)
  install(FILES ${olive_EFFECTS} DESTINATION share/olive-editor/effects)
  install(FILES packaging/linux/org.olivevideoeditor.Olive.desktop DESTINATION share/applications)
  install(FILES packaging/linux/org.olivevideoeditor.Olive.appdata.xml DESTINATION share/metainfo)
  install(FILES packaging/linux/org.olivevideoeditor.Olive.xml DESTINATION share/mime/packages)
  install(FILES packaging/linux/icons/16x16/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/16x16/apps)
  install(FILES packaging/linux/icons/32x32/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/32x32/apps)
  install(FILES packaging/linux/icons/48x48/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/48x48/apps)
  install(FILES packaging/linux/icons/64x64/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/64x64/apps)
  install(FILES packaging/linux/icons/128x128/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/128x128/apps)
  install(FILES packaging/linux/icons/256x256/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/256x256/apps)
  install(FILES packaging/linux/icons/512x512/org.olivevideoeditor.Olive.png DESTINATION share/icons/hicolor/512x512/apps)
  install(FILES ${QM_FILES} DESTINATION share/olive-editor/ts)
endif()
