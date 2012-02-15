all:: bem-bl 
all:: $(patsubst %.bemjson.js,%.html,$(wildcard pages/*/*.bemjson.js))

BEM_BUILD=bem build \
	-l bem-mini/ \
	-l blocks/ \
	-d $< \
	-t $1 \
	-o $(@D) \
	-n $(*F)

BEM_CREATE=bem create block \
		-l pages \
		-t $1 \
		$(*F)

LESS_BUILD= lessc $< $(@D)/$(*F).css

%.html: %.bemhtml.js %.js %.lessbuild
	$(call BEM_CREATE,bem-bl/blocks-common/i-bem/bem/techs/html.js)

%.bemhtml.js: %.deps.js
	$(call BEM_BUILD,bem-bl/blocks-common/i-bem/bem/techs/bemhtml.js)

%.deps.js: %.bemdecl.js
	$(call BEM_BUILD,deps.js)
	if [ -e $(@D)/$(*F).html ]; then rm $(@D)/$(*F).html; fi
	if [ -e $(@D)/$(*F).less ]; then rm $(@D)/$(*F).less; fi
	if [ -e $(@D)/$(*F).js ]; then rm $(@D)/$(*F).js; fi

%.bemdecl.js: %.bemjson.js
	$(call BEM_CREATE,bemdecl.js)

.PRECIOUS: %.css
%.css: %.deps.js
	$(call BEM_BUILD,css)
	borschik -t css -i $(@D)/$(*F).css -o $(@D)/_$(*F).css

.PRECIOUS: %.ie.css
%.ie.css: %.deps.js
	$(call BEM_BUILD,ie.css)
	borschik -t css -i $(@D)/$(*F).ie.css -o $(@D)/_$(*F).ie.css

.PRECIOUS: %.ie.less
%.ie.less: %.deps.js
	$(call BEM_BUILD,ie.less)

.PRECIOUS: %.less
%.less: %.deps.js
	$(call BEM_BUILD,less)

.PRECIOUS: %.lessbuild
%.lessbuild: %.less
	$(call LESS_BUILD,less,css)

.PRECIOUS: %.js
%.js: %.deps.js
	$(call BEM_BUILD,js)


DO_GIT=@echo -- git $1 $2; \
	if [ -d $2 ]; \
		then \
			cd $2 && git pull origin master; \
		else \
			git clone $1 $2; \
	fi

bem-bl:
	$(call DO_GIT,git://github.com/bem/bem-bl.git,$@)


.PHONY: all
