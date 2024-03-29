DIR=bibxml/bibxml-w3c
WEB=/home/www/tools.ietf.org/tools/xml2rfc/rfcs/$(DIR)

SHFILES=gen-bibxml-w3c-all
PYFILES=gen-bibxml-w3c
EXEFILES= $(SHFILES) $(PYFILES)
OTHERFILES=makefile

none:
	@echo make all
	@echo make production
	@echo make install -- install local files into production
	@echo make clean -- clean local files into production
	@echo
	@echo Test targets:
	@echo make test -- unit test local files
	@echo make pylint -- run pylint on python scripts
	@echo
	@echo make getxls
	@echo make geturl
	@echo make dumpjson
	@echo make check-removals
	@echo make do-removals
	@echo make gen-xmls

URL= http://tlhansen.us/ietf/get-w3c.php
URL= https://www.w3.org/2002/01/tr-automation/tr.rdf
DEFDIR=bibxml-w3c-new
PRODDIR="/home/www/tools.ietf.org/tools/xml2rfc/web/public/rfc/bibxml-w3c-new"

# download the spreadsheet and then process it
all:
	./gen-bibxml-w3c -v -u $(URL) -f /tmp/w.rdf -b $(DEFDIR) -c

production:
	./gen-bibxml-w3c -v -u $(URL) -b $(PRODDIR) -c

test:
	chmod a+rx $(PYFILES) $(SHFILES)
	for i in $(PYFILES); do "./$$i" --unit-tests; done
	for i in $(SHFILES); do bash -n "$$i"; done
	for i in $(PYFILES); do "./$$i" --help; done

pylint:
	PYTHONPATH=../bibxml_common pylint --rcfile=../.pylintrc $(PYFILES)

install:
	@for i in $(EXEFILES); do \
	    if [ $$i -nt $(WEB)/$$i -o ! -f $(WEB)/$$i ];then \
	        echo cp -p $$i $(WEB)/$$i; \
	        chmod a+rx $$i && cp -p $$i $(WEB)/$$i ; \
	    fi; \
	done
	@for i in $(OTHERFILES); do \
	    if [ $$i -nt $(WEB)/$$i -o ! -f $(WEB)/$$i ];then \
	        echo cp -p $$i $(WEB)/$$i; \
	        chmod a+r,a-x $$i && cp -p $$i $(WEB)/$$i ; \
	    fi; \
	done

clean:
	rm -f *~

test-all:
	cd .. && $(MAKE) test
clean-all:
	cd .. && $(MAKE) clean
install-all:
	cd .. && $(MAKE) install

# TESTS

all-but-rdf:
	./gen-bibxml-w3c -u $(URL) -b $(DEFDIR) -c -v -j /tmp/w.json

# download the spreadsheet
geturl:
	./gen-bibxml-w3c -u $(URL) -f /tmp/w.rdf

# convert the spreadsheet to a readable json file
dumpjson:
	./gen-bibxml-w3c -f /tmp/w.rdf -j /tmp/w.json

# convert the spreadsheet to a readable yaml file
dumpyaml:
	./gen-bibxml-w3c -f /tmp/w.rdf -y /tmp/w.yaml

# convert the spreadsheet to a readable csv file
dumpcsv:
	./gen-bibxml-w3c -f /tmp/w.rdf -C /tmp/w.csv

# show which files would be removed, when not in the spreadsheet
check_removals:
	./gen-bibxml-w3c -f /tmp/w.rdf -c -s -b $(DEFDIR) -S

# remove any files not in the spreadsheet
do_removals:
	./gen-bibxml-w3c -f /tmp/w.rdf -c -s -b $(DEFDIR)

# update the bibxml-w3c directory based on the spreadsheet
gen-xmls:
	./gen-bibxml-w3c -v -f /tmp/w.rdf -b $(DEFDIR) -c

# same as above, adding -v
gen-xmls-v:
	./gen-bibxml-w3c -f /tmp/w.rdf -b $(DEFDIR) -c -v

