CFLAGS=$(shell /usr/bin/python3-config --cflags --embed)
LDFLAGS=$(shell /usr/bin/python3-config --ldflags --embed)

all: exemplo

pyversion.h:
	python3 -c "import sys; print(f'#define PY_TAG \"{sys.version_info.major}.{sys.version_info.minor}\"')" > pyversion.h

exemplo: main.c pyversion.h
	gcc -o exemplo $^ $(CFLAGS) $(LDFLAGS) -fPIE

clean:
	rm -rf ../exemplo-0.1.0/usr/bin
	rm -f exemplo pyversion.h

install: all
	mkdir -p ../exemplo-0.1.0/usr/bin
	cp exemplo ../exemplo-0.1.0/usr/bin

.PHONY: all clean install
