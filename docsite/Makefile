#!/usr/bin/make
SITELIB = $(shell python -c "from distutils.sysconfig import get_python_lib; print get_python_lib()")
FORMATTER=../hacking/module_formatter.py

all: clean docs

docs: clean modules staticmin
	./build-site.py
	-(cp *.ico htmlout/)
	-(cp *.jpg htmlout/)
	-(cp *.png htmlout/)

viewdocs: clean staticmin
	./build-site.py view

htmldocs: staticmin
	./build-site.py rst

clean:
	-rm -rf htmlout
	-rm -f .buildinfo
	-rm -f *.inv
	-rm -rf *.doctrees
	@echo "Cleaning up minified css files"
	find . -type f -name "*.min.css" -delete
	@echo "Cleaning up byte compiled python stuff"
	find . -regex ".*\.py[co]$$" -delete
	@echo "Cleaning up editor backup files"
	find . -type f \( -name "*~" -or -name "#*" \) -delete
	find . -type f \( -name "*.swp" \) -delete

.PHONEY: docs clean

modules: $(FORMATTER) ../hacking/templates/rst.j2
	PYTHONPATH=../lib $(FORMATTER) -t rst --template-dir=../hacking/templates --module-dir=../library -o rst/

staticmin:
	cat _themes/srtd/static/css/theme.css | sed -e 's/^[ \t]*//g; s/[ \t]*$$//g; s/\([:{;,]\) /\1/g; s/ {/{/g; s/\/\*.*\*\///g; /^$$/d' | sed -e :a -e '$$!N; s/\n\(.\)/\1/; ta' > _themes/srtd/static/css/theme.min.css
