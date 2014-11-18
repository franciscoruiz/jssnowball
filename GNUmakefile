# -*- makefile -*-

libstemmer_algorithms = armenian basque catalan czech danish dutch english \
			finnish french german hungarian italian irish \
			norwegian porter portuguese romanian \
			russian spanish slovene swedish turkish

java_src_generated = build/snowball_java_src/libstemmer_java/java/org/tartarus/snowball/ext
JAVA_SOURCES = $(libstemmer_algorithms:%=$(java_src_generated)/%Stemmer.java)
JS_TESTS_SRC = $(libstemmer_algorithms:%=tests/js/%Tests.js)
JS_TESTS_HTML = $(libstemmer_algorithms:%=tests/%Tests.html)

all: tests/index.html tests/composite.html dist/Snowball.js

tests/index.html: $(JAVA_SOURCES)
	@echo "<!DOCTYPE html>" > $@
	@echo "<html>" >> $@
	@echo "<head>" >> $@
	@echo "<meta charset=\"utf-8\">" | sed 's!^!\t!' >> $@
	@echo "<title>Online Snowball stemmers demo</title>" | sed 's!^!\t!' >> $@
	@echo "<script src=\"../dist/Snowball.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "<script>" | sed 's!^!\t!' >> $@
	@echo "var Stem = (function() { var lang, testStemmer; return function(lng, word) {"	\
              "if (lng != lang) {lang = lng; testStemmer = new Snowball(lang);} "		\
	      "testStemmer.setCurrent(word); testStemmer.stem();"				\
	      "return testStemmer.getCurrent();}})();" | sed 's!^!\t\t!' >> $@
	@echo "function printstem(lng, word){document.getElementById(\"result\").innerHTML"	\
              "= \"<b>\" + Stem(lng, word) + \"</b>\";}" | sed 's!^!\t\t!' >> $@
	@echo "</script>" | sed 's!^!\t!' >> $@
	@echo "</head>" >> $@
	@echo "<body>" >> $@
	@echo "<p>Type ONE word, select language and press \"<b>Stem!</b>\" button.</p>" | sed 's!^!\t!' >> $@
	@echo "<input maxlength=\"50\" size=\"50\" id=\"query\" type=\"text\">" | sed 's!^!\t!' >> $@
	@echo "<select id=\"language\">" | sed 's!^!\t!' >> $@
	@$(foreach l,$(libstemmer_algorithms), echo "<option value=\"$(l)\">$(l)</option>" | sed 's!^!\t\t!' >> $@;)
	@echo "</select>" | sed 's!^!\t!' >> $@
	@echo "<button type=\"button\" onclick=\""						\
	      "printstem(document.getElementById('language').value, "				\
	      "document.getElementById('query').value);\"><noscript>"				\
	      "<span style=\"color:red;\">Enable JavaScript ! </span></noscript>Stem!</button>" | sed 's!^!\t!' >> $@
	@echo "<p id=\"result\"></p>" | sed 's!^!\t!' >> $@
	@echo "<fieldset><legend>Unit tests</legend>" | sed 's!^!\t!' >> $@
	@echo "<a href=\"tests/composite.html\" target=\"_blank\">all</a>" | sed 's!^!\t\t!' >> $@;
	@$(foreach l,$(libstemmer_algorithms), echo "<a href=\"tests/$(l)Tests.html\" target=\"_blank\">$(l)</a>" | sed 's!^!\t\t!' >> $@;)
	@echo "</fieldset><p></p>" | sed 's!^!\t!' >> $@
	@echo "<fieldset><legend>Links</legend>" | sed 's!^!\t!' >> $@
	@echo "<a href=\"https://github.com/mazko/jssnowball\" target=\"_blank\" rel=\"nofollow\">Sources</a> |" | sed 's!^!\t\t!' >> $@;
	@echo "<a href=\"http://snowball.tartarus.org/\" target=\"_blank\" rel=\"nofollow\">Snowball</a> |" | sed 's!^!\t\t!' >> $@;
	@echo "<a href=\"http://mazko.github.com/jsli/\" target=\"_blank\">Language Identifier</a> |" | sed 's!^!\t\t!' >> $@;
	@echo "<a href=\"http://autotagcloud.com/\" target=\"_blank\">AutoTagCloud</a> |" | sed 's!^!\t\t!' >> $@;
	@echo "<a href=\"mailto:o.mazko%20%5Bat%5D%20mail.ru?subject=Snowball%20to%20JavaScript%20generator\" " \
	      "rel=\"nofollow\" onmouseover=\"var split = this.href.split('?'); "				\
	      "split[0] = split[0].replace('%20%5Bat%5D%20','@'); this.href = split.join('?');\">Feedback</a>" | sed 's!^!\t\t!' >> $@;
	@echo "</fieldset>" | sed 's!^!\t!' >> $@
	@echo "</body>" >> $@
	@echo "</html>" >> $@

tests/composite.html: $(JS_TESTS_SRC) $(JS_TESTS_HTML)
	@echo "<!DOCTYPE html>" > $@
	@echo "<html>" >> $@
	@echo "<head>" >> $@
	@echo "<meta charset=\"utf-8\">" | sed 's!^!\t!' >> $@
	@echo "<title>QUnit tests for all stemmers</title>" | sed 's!^!\t!' >> $@
	@echo "<link rel=\"stylesheet\" href=\"qunit/qunit-1.10.0.css\">" | sed 's!^!\t!' >> $@
	@echo "<link rel=\"stylesheet\" href=\"qunit/addons/composite/qunit-composite.css\">" | sed 's!^!\t!' >> $@
	@echo "<script src=\"qunit/qunit-1.10.0.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "<script src=\"qunit/addons/composite/qunit-composite.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "<script>" | sed 's!^!\t!' >> $@
	@echo "QUnit.config.hidepassed = true;" | sed 's!^!\t\t!' >> $@
	@echo "QUnit.testSuites([" | sed 's!^!\t\t!' >> $@
	@$(foreach lang,$(libstemmer_algorithms), 						\
	echo $${separator_between_tests} >> $@; separator_between_tests=","; 			\
	echo "\"$(lang)Tests.html\"" | sed 's!^!\t\t\t!' >> $@;)
	@echo "]);" | sed 's!^!\t\t!' >> $@
	@echo "</script>" | sed 's!^!\t!' >> $@
	@echo "</head>" >> $@
	@echo "<body>" >> $@
	@echo "<div id=\"qunit\"></div>" | sed 's!^!\t!' >> $@
	@echo "<div id=\"qunit-fixture\"></div>" | sed 's!^!\t!' >> $@
	@echo "</body>" >> $@
	@echo "</html>" >> $@

tests/%Tests.html:
	@echo "<!DOCTYPE html>" > $@
	@echo "<html>" >> $@
	@echo "<head>" >> $@
	@echo "<meta charset=\"utf-8\">" | sed 's!^!\t!' >> $@
	@echo "<title>QUnit tests for $* stemmer</title>" | sed 's!^!\t!' >> $@
	@echo "<link rel=\"stylesheet\" href=\"qunit/qunit-1.10.0.css\">" | sed 's!^!\t!' >> $@
	@echo "</head>" >> $@
	@echo "<body>" >> $@
	@echo "<div id=\"qunit\"></div>" | sed 's!^!\t!' >> $@
	@echo "<script src=\"qunit/qunit-1.10.0.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "<script src=\"../dist/Snowball.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "<script src=\"js/$*Tests.js\"></script>" | sed 's!^!\t!' >> $@
	@echo "</body>" >> $@
	@echo "</html>" >> $@

tests/js/%Tests.js: build/snowball_all/algorithms/%/voc.txt build/snowball_code/stemwords
	@mkdir -p tests/js
	@echo "QUnit.config.hidepassed = true;" > $@
	@echo "QUnit.config.blocking = false;" >> $@
	@echo "var Stem = (function() { var testStemmer = new Snowball('$*'); return function(word) { "		\
              "testStemmer.setCurrent(word); testStemmer.stem(); return testStemmer.getCurrent();}})();" >> $@
	@echo "Generating tests for $*"
	@./build/snowball_code/stemwords -i build/snowball_all/algorithms/$*/voc.txt -l $* -p | 	 			\
	       sed '/^\s\+\S*\s\+$$/d' | sed 's!\"!\\\"!g' | 							\
	       sed 's!\s\+[->]\+\s\+!\"\), \"!' |								\
	       sed 'h;G;s/\n/\", function\(\) {deepEqual\( Stem(\"/' | 						\
	       sed 's!\"), \"! -> !' |										\
	       sed 's!^!test\(\"!' | sed 's!$$!\"\);}\);!' >> $@
	@total=`cat build/snowball_all/algorithms/$*/voc.txt | sed '/^$$/d' | wc -l`;					\
	echo "QUnit.done(function( details ) {" 								\
	     "test(\"Total tests generated equals total words count in voc.txt\", "				\
	     "function() {deepEqual(details.total, $${total}); QUnit.config.done = []});});" >> $@

build/snowball_code/stemwords: $(JAVA_SOURCES)
	@cp build/snowball_code/GNUmakefile build/snowball_code/GNUmakefile_js_copy
	@cp build/snowball_code/libstemmer/modules.txt build/snowball_code/libstemmer/modules_js_copy.txt
	@sed -i 's!libstemmer\/modules\.txt!libstemmer\/modules_js_copy\.txt!' build/snowball_code/GNUmakefile_js_copy
	@$(foreach a,$(libstemmer_algorithms), grep -q '\s*$(a)\s\+' build/snowball_code/libstemmer/modules_js_copy.txt || \
	echo '$(a) UTF_8 $(a)' >> build/snowball_code/libstemmer/modules_js_copy.txt;)
	@make -C build/snowball_code libstemmer_algorithms="$(subst $(eval), ,$(libstemmer_algorithms))" -f GNUmakefile_js_copy --no-print-directory stemwords

dist/Snowball.js: $(JAVA_SOURCES) $(wildcard src/*.js)
	@mkdir -p dist
	@echo "/*!" > $@
	@echo " * Snowball JavaScript Library v0.4" >> $@
	@echo " * http://snowball.tartarus.org/" >> $@
	@echo " * https://github.com/mazko/jssnowball" >> $@
	@echo " *" >> $@
	@echo " * Copyright `date +'%y.%m.%d %H:%M:%S'`, Oleg Mazko" >> $@
	@echo " * http://www.opensource.org/licenses/bsd-license.html" >> $@
	@echo " */" >> $@
	@echo "function Snowball(lng) {" >> $@
	@cat src/Among.js | sed '1,9d' | sed 's!^!\t!' >> $@
	@cat src/SnowballProgram.js | sed '1,9d' | sed 's!^!\t!' >> $@
	@echo "var stemFactory = {" | sed 's!^!\t!' >> $@
	@$(foreach dir,$(libstemmer_algorithms), 							\
	echo $${separator_between_stemmers} >> $@; separator_between_stemmers=","; 			\
	echo "$(dir)Stemmer : function() {" | sed 's!^!\t\t!' | sed 's!$$!\n!' >> $@;			\
	cat $(java_src_generated)/$(dir)Stemmer.java | sed '1,11d' | sed 's!^!\t\t!' >> $@;)
	@echo "}" | sed 's!^!\n\t!' >> $@;
	@echo "var stemName = lng.toLowerCase() + \"Stemmer\";" | sed 's!^!\t!' >> $@
	@echo "return new stemFactory[stemName]();" | sed 's!^!\t!' >> $@
	@echo "}" >> $@
	@grep -q 'copy_from\|in_range\|in_range_b\|out_range\|out_range_b\|assign_to\|eq_v[^_]' $@ && \
	echo 'Error: possible usage of not implemented method in SnowballProgramm.js' ; [ $$? -ne 0 ]

build/snowball_code/algorithms/%/stem_Unicode.sbl: build/snowball_code/algorithms/%/stem_ISO_8859_1.sbl
	cp $^ $@

$(java_src_generated)/%Stemmer.java: build/snowball_code/algorithms/%/stem_Unicode.sbl \
	    $(wildcard build/snowball_code/compiler/*.c) $(wildcard build/snowball_code/compiler/*.h)
	@target=`echo "$@" | sed 's![^/]*/!!'`; \
	make -C build/snowball_code --no-print-directory $${target}

# Environment test - generate java sources and compare with downloaded. Used in ./configure

java_src_check: $(JAVA_SOURCES)
	diff -r $(JAVA_SRC_DWNLD)/libstemmer_java/$(java_src_generated) $(java_src_generated)

clean:
	-make -C build/snowball_code -f GNUmakefile_js_copy libstemmer_algorithms="$(subst $(eval), ,$(libstemmer_algorithms))" --no-print-directory clean
	-rm dist \
		tests/js/*Tests.js tests/composite.html tests/*Tests.html tests/index.html \
		build
