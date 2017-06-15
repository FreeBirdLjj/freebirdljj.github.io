---
layout: post
title:  "Implied Rules of `make`"
date:   2017-06-14 23:00:00 +0000
categories: jekyll update
---
`make` 有一些隐含的（默认的）推导规则，掌握这些隐含规则不仅能帮我们简化 Makefile 的编写，也能避免我们写出的 Makefile 在 `make` 时出现奇怪的现象。

```make
%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c -o $@ $?

%.o: %.C
	$(CXX) $(CFLAGS) $(CPPFLAGS) -c -o $@ $?

%.o: %.CC
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -c -o $@ $?

%.o: %.p
	$(PC) -c $(PFLAGS) $? -o $@

%.o: %.f
	$(FC) -c $(FFLAGS) $? -o $@

%.f: %.F
	$(FC) -F $(CPPFLAGS) $(FFLAGS) $? -o $@

%.f: %.r
	$(FC) -F $(FFLAGS) $(RFLAGS) $? -o $@

%.o: %.F
	$(FC) -c $(FFLAGS) $(CPPFLAGS) $? -o $@

%.o: %.r
	$(FC) -c $(FFLAGS) $(RFLAGS) $? -o $@

%.sym: %.def
	$(M2C) $(M2FLAGS) $(DEFFLAGS) $? -o $@

%.o: %.mod
	$(M2C) $(M2FLAGS) $(MODFLAGS) $? -o $@

%.o: %.s
	$(AS) $(ASFLAGS) $? -o $@

%.s: %.S
	$(CC) -E $(CPPFLAGS) $? > $@

%: %.o
	$(CC) $(LDFLAGS) $? $(LOADLIBES) $(LDLIBS) -o $@

%.c: %.y
	$(YACC) $(YFLAGS) $?
	mv -f y.tab.c %@

%.c: %.l
	$(LEX) $(LFLAGS) -t $? > $@
```

