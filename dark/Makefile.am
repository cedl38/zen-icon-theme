themedir = ${datadir}/icons/Zen-dark

THEME_IN_FILES = index.theme.in

@INTLTOOL_THEME_RULE@

theme_DATA = index.theme

EXTRA_DIST=				\
	$(THEME_IN_FILES)

install-data-am: install-data-local install-themeDATA
	($(abs_top_builddir)/index.theme.sh $(DESTDIR)$(themedir))

install-data-local:
	echo -e "Make dark icons files..."; \
	$(RECOLOR) ../src -negate -modulate 92 -p dark.dat "."; \
	for size in 16x16 22x22 32x32; do \
		echo -e "Converting $$size svg icons to $$size png..."; \
		cd $$size && \
		for dir in `find * -type d`; do \
			$(mkdir_p) $(DESTDIR)$(themedir)/$$size/$$dir; \
		done; \
		for icon in `find * -name "*.svg"`; do \
			pngpath="$(DESTDIR)$(themedir)/$$size/$${icon%.*}.png"; \
			i=$${size%x*}; \
			$(SVGCONVERT) $$icon -w $$i -h $$i -o $$pngpath; \
		done; \
		cd ..; \
	done; \
	for size_svg in 48x48; do \
		echo -e "Copying $$size_svg icons to scalable..."; \
		cd $$size_svg && \
			for dir in `find * -type d`; do \
				$(mkdir_p) $(DESTDIR)$(themedir)/scalable/$$dir; \
			done; \
			for icon in `find * -name "*.svg"`; do \
				$(install_sh_DATA) $$icon $(DESTDIR)$(themedir)/scalable/$$icon; \
			done; \
		cd ..; \
	done;
if ENABLE_ICON_FRAMING
	echo -e "Converting dark icons files from 22x22 to $(FRAMING_SIZE)..."; \
	cd $(DESTDIR)$(themedir); \
	cd 22x22 && \
	for dir in `find * -type d`; do \
		$(mkdir_p) ../$(FRAMING_SIZE)/$$dir; \
	done; \
	for icon in `find * -name "*.png"`; do \
		$(CONVERT) $$icon ../$(FRAMING_SIZE)/$$icon; \
	done; \
	cd ..;
endif
	if [ $(ICONMAP) != "false" ]; then \
	cd $(DESTDIR)$(themedir); \
	for size in 16x16 22x22 32x32 $(FRAMING_SIZE) scalable $(LARGE_SIZES); do \
		cd $$size || continue; \
		echo -e "Set up icon mapping for $$size..."; \
		for dir in `find . -type d`; do \
			context="`echo $$dir | cut -c 3-`"; \
			if [ $$context ]; then \
				$(ICONMAP) -c $$context; \
			fi; \
		done; \
		cd ..; \
	done; \
	fi;
