cmake_minimum_required(VERSION 2.6)
if(COMMAND cmake_policy)
  cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

project(Ensipong)
enable_testing()


#######
# Detect SDL using standard CMake FindSDL
#####
find_package(SDL REQUIRED)
set(SDL_INCLUDE_CFLAGS -I${SDL_INCLUDE_DIR})


# Debug build

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} ${SDL_INCLUDE_CFLAGS} -Wall -Werror -std=gnu11")

#########
# Vous devez editer les lignes suivantes pour y mettre vos logins
#########
set(VARIANTE_LOGINS login1 login2 login3)

#########

list(SORT VARIANTE_LOGINS)
if (VARIANTE_LOGINS MATCHES "login[123]")
  message(FATAL_ERROR "** ERREUR **: Vous devez modifier CMakeLists.txt pour y mettre vos logins")
endif()

#########

##
# Si vous utilisez plusieurs fichiers, en plus de shmem_pong.c, pour votre
# shell il faut les ajouter ici
##
add_executable(ensipong src/shmem_pong.c)
target_link_libraries(ensipong ${SDL_LIBRARY} -lrt)

##
# Programme de test
##
add_test(pong ../tests/fourpong.pl)

##
# Ajout d'une cible pour lancer les tests de manière verbeuse
##
add_custom_target(check ctest -V)
