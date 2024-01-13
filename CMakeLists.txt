cmake_minimum_required(VERSION 3.20)
project(anti-grain-geometry)

set(AGG_MAJOR_VERSION 2)
set(AGG_MINOR_VERSION 7)
set(AGG_BUILD_VERSION 1)

option(AGG_USE_GPC "Use GeneralPolygonClipper library" OFF)
option(AGG_USE_FREETYPE "Use freetype library" OFF)
option(AGG_USE_EXPAT "Use expat library" OFF)
option(AGG_USE_SDL_PLATFORM "Use SDL as platform" OFF)
option(AGG_USE_PACK "Packaged Agg" OFF)
option(AGG_USE_AGG2D "Agg 2D graphical context" OFF)
option(AGG_BUILD_EXAMPLE "Build example" OFF)

set(CMAKE_DEBUG_POSTFIX "d")


##################################################
# Set all includes, flags, libraries, related to expat
##################################################

IF( AGG_USE_EXPAT )

    FIND_PACKAGE( EXPAT )                    

    IF(EXPAT_FOUND)  
        INCLUDE_DIRECTORIES(${EXPAT_INCLUDE_DIRS})
        LINK_LIBRARIES(${EXPAT_LIBRARIES})
    ELSE(EXPAT_FOUND)  
        MESSAGE(SEND_ERROR "expat not found")
    ENDIF(EXPAT_FOUND)
ENDIF( AGG_USE_EXPAT )

   
##################################################
# Set all includes, flags, libraries, related to freetype
##################################################

IF( AGG_USE_FREETYPE )
    FIND_PACKAGE( Freetype )                    
    IF( FREETYPE_FOUND )
        INCLUDE_DIRECTORIES( ${FREETYPE_INCLUDE_DIRS} )
        LINK_LIBRARIES( ${FREETYPE_LIBRARIES} )
        LINK_DIRECTORIES( ${FREETYPE_LINK_DIR} )		
    ELSE( FREETYPE_FOUND )
        MESSAGE(SEND_ERROR "freetype not found")
    ENDIF( FREETYPE_FOUND )
ENDIF( AGG_USE_FREETYPE )


##################################################
# Set all includes, flags, libraries, related to SDL
##################################################

FIND_PACKAGE( SDL QUIET )
IF( SDL_FOUND )
    IF ( AGG_USE_SDL_PLATFORM )
        INCLUDE_DIRECTORIES(${SDL_INCLUDE_DIR})
        LINK_LIBRARIES(${SDL_LIBRARY})
    ENDIF ( AGG_USE_SDL_PLATFORM )
ELSE( SDL_FOUND )
    IF ( agg_USE_SDL_PLATFORM )
        MESSAGE( "SDL libray was not found, disable AGG_USE_SDL_PLATFORM please" )
    ENDIF ( AGG_USE_SDL_PLATFORM )
ENDIF( SDL_FOUND )


include_directories(${CMAKE_SOURCE_DIR}/include)

add_subdirectory(src)
if(AGG_BUILD_EXAMPLE)
add_subdirectory(example)
endif()