This is libgit2 from here: https://github.com/libgit2/libgit2

cmake -DBUILD_SHARED_LIBS=OFF -DUSE_SSH=OFF -DBUILD_CLAR=OFF -D..
make DESTDIR=${PWD}/install install
