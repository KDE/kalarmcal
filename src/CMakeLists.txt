add_definitions(-DTRANSLATION_DOMAIN=\"libkalarmcal5\")
########### next target ###############
add_library(KF5AlarmCalendar)
add_library(KF5::AlarmCalendar ALIAS KF5AlarmCalendar)

target_sources(KF5AlarmCalendar PRIVATE
    kacalendar.cpp
    karecurrence.cpp
    akonadi.cpp
    alarmtext.cpp
    identities.cpp
    kaevent.cpp
    kadatetime.cpp
    datetime.cpp
    repetition.cpp
    version.cpp
    collectionattribute.cpp
    compatibilityattribute.cpp
    eventattribute.cpp
)

ecm_qt_declare_logging_category(KF5AlarmCalendar HEADER kalarmcal_debug.h IDENTIFIER KALARMCAL_LOG CATEGORY_NAME org.kde.pim.kalarmcal
        DESCRIPTION "kalarmcal (kalarmcal)"
        OLD_CATEGORY_NAMES log_kalarmcal
        EXPORT KALARMCAL
    )
if (COMPILE_WITH_UNITY_CMAKE_SUPPORT)
    set_target_properties(KF5AlarmCalendar PROPERTIES UNITY_BUILD ON)
endif()

generate_export_header(KF5AlarmCalendar BASE_NAME kalarmcal)



target_link_libraries(KF5AlarmCalendar
PUBLIC
                      KF5::ConfigCore
                      KF5::I18n
                      KF5::Holidays
                      KF5::IdentityManagement
                      KF5::CoreAddons
                      KF5::CalendarCore
                      KF5::AkonadiCore
PRIVATE
)

set_target_properties(KF5AlarmCalendar PROPERTIES
      VERSION ${KALARMCAL_VERSION}
      SOVERSION ${KALARMCAL_SOVERSION}
      EXPORT_NAME AlarmCalendar
)


target_include_directories(KF5AlarmCalendar INTERFACE "$<INSTALL_INTERFACE:${KDE_INSTALL_INCLUDEDIR_KF5}/KAlarmCal>")
target_include_directories(KF5AlarmCalendar PUBLIC "$<BUILD_INTERFACE:${KAlarmCal_SOURCE_DIR}/src;${KAlarmCal_BINARY_DIR}/src>")


install(TARGETS KF5AlarmCalendar EXPORT KF5AlarmCalendarTargets ${KF5_INSTALL_TARGETS_DEFAULT_ARGS})


ecm_generate_headers(KAlarmCal_CamelCase_HEADERS
  HEADER_NAMES
  Akonadi
  AlarmText
  CollectionAttribute
  CompatibilityAttribute
  DateTime
  EventAttribute
  Identities
  KACalendar
  KADateTime
  KAEvent
  KARecurrence
  Repetition
  Version
  PREFIX KAlarmCal
  REQUIRED_HEADERS KAlarmCal_HEADERS
)


install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/kalarmcal_export.h
        ${KAlarmCal_HEADERS}
        DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5}/KAlarmCal/kalarmcal
        COMPONENT Devel
)
install(FILES
        ${KAlarmCal_CamelCase_HEADERS}
        DESTINATION ${KDE_INSTALL_INCLUDEDIR_KF5}/KAlarmCal/KAlarmCal/
        COMPONENT Devel
)


ecm_generate_pri_file(BASE_NAME KAlarmCal LIB_NAME KF5AlarmCalendar DEPS "KHolidays KIdentityManagement KCoreAddons KCalendarCore AkonadiCore" FILENAME_VAR PRI_FILENAME INCLUDE_INSTALL_DIR ${KDE_INSTALL_INCLUDEDIR_KF5}/KAlarmCal/)
install(FILES ${PRI_FILENAME} DESTINATION ${ECM_MKSPECS_INSTALL_DIR})
