PYTHON=python

EXTRA_DIST=                      \
 AUTHORS                         \
 COPYING                         \
 MANIFEST.in                     \
 emanesh/events/antennaprofileevent.py \
 emanesh/events/commeffectevent.py \
 emanesh/events/event.py \
 emanesh/events/eventserviceexception.py \
 emanesh/events/eventservice.py \
 emanesh/events/__init__.py \
 emanesh/events/locationevent.py \
 emanesh/events/onehopneighborsevent.py \
 emanesh/events/pathlossevent.py \
 emanesh/controlportclient.py    \
 emanesh/controlportexception.py \
 emanesh/__init__.py             \
 emanesh/emaneshell.py           \
 emanesh/manifest.py             \
 emanesh/manifestexception.py    \
 emanesh/schema                  \
 scripts/emanesh                 \
 scripts/emaneevent-dump         \
 scripts/emaneevent-pathloss     \
 setup.py.in                     \
 stdeb.cfg.in

BUILT_SOURCES =                  \
 emanesh/events/antennaprofileevent_pb2.py \
 emanesh/events/commeffectevent_pb2.py  \
 emanesh/events/event_pb2.py            \
 emanesh/events/locationevent_pb2.py    \
 emanesh/events/pathlossevent_pb2.py    \
 emanesh/remotecontrolportapi_pb2.py \
 emanesh/events/onehopneighborsevent_pb2.py \
 emanesh/events/otaheader_pb2.py \
 emanesh/schema/manifest.xsd \
 setup.py

all-local:
	$(PYTHON) setup.py build

clean-local: setup.py
	$(PYTHON) setup.py clean
	-rm -rf build
	-rm -f MANIFEST
	-rm -f emanesh/events/antennaprofileevent_pb2.py
	-rm -f emanesh/events/commeffectevent_pb2.py
	-rm -f emanesh/events/event_pb2.py
	-rm -f emanesh/events/locationevent_pb2.py
	-rm -f emanesh/events/pathlossevent_pb2.py
	-rm -f emanesh/remotecontrolportapi_pb2.py
	-rm -f emanesh/events/onehopneighborsevent_pb2.py
	-rm -f emanesh/events/otaheader_pb2.py
	-rm -f emanesh/schema/manifest.xsd
	-rm -rf dist
	-rm -rf deb_dist
	-rm -f stdeb.cfg
	-rm -r emanesh/schema
	-find . -name "*.pyc" -delete

DISTCLEANFILES=setup.py

sdist:
	$(PYTHON) setup.py sdist

edit = sed                              \
        -e 's|@VERSION[@]|$(VERSION)|g' \
        -e 's|@PYTHON[@]|$(PYTHON)|g'   \
        -e 's|@DEBIAN_VERSION[@]|$(DEBIAN_VERSION)|g'

emanesh/remotecontrolportapi_pb2.py: ../libemane/remotecontrolportapi.proto
	protoc -I../libemane --python_out=emanesh  $<

emanesh/events/event_pb2.py: ../libemane/event.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/locationevent_pb2.py: ../libemane/locationevent.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/otaheader_pb2.py: ../libemane/otaheader.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/pathlossevent_pb2.py: ../libemane/pathlossevent.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/commeffectevent_pb2.py: ../libemane/commeffectevent.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/antennaprofileevent_pb2.py: ../libemane/antennaprofileevent.proto
	protoc -I../libemane --python_out=emanesh/events  $<

emanesh/events/onehopneighborsevent_pb2.py: ../models/mac/ieee80211abg/onehopneighborsevent.proto
	protoc -I../models/mac/ieee80211abg --python_out=emanesh/events  $<

emanesh/schema/manifest.xsd: @top_srcdir@/schema/manifest.xsd	emanesh/schema
	cp $< $@

emanesh/schema:
	mkdir -p emanesh/schema

setup.py:	setup.py.in
	if test -f $@; then chmod u+w $@; fi
	$(edit) $< > $@
	chmod g-w,u-w $@

rpm: $(BUILT_SOURCES)
	$(PYTHON) setup.py bdist_rpm  \
   --vendor "Adjacent Link LLC" \
   --requires "protobuf-python python-setuptools"

stdeb.cfg: stdeb.cfg.in
	if test -f $@; then chmod u+w $@; fi
	$(edit) $< > $@
	chmod g-w,u-w $@

deb: $(BUILT_SOURCES) stdeb.cfg
	$(PYTHON) setup.py --command-packages=stdeb.command bdist_deb