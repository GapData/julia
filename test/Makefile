SRCDIR := $(abspath $(dir $(lastword $(MAKEFILE_LIST))))
JULIAHOME := $(abspath $(SRCDIR)/..)
BUILDDIR := .
include $(JULIAHOME)/Make.inc
# TODO: this Makefile ignores BUILDDIR, except for computing JULIA_EXECUTABLE

TESTGROUPS = linalg sparse unicode strings dates
TESTS = all $(TESTGROUPS) \
		$(filter-out TestHelpers runtests testdefs, \
			$(patsubst $(SRCDIR)/%.jl,%,$(wildcard $(SRCDIR)/*.jl))) \
		$(foreach group,$(TESTGROUPS), \
			$(patsubst $(SRCDIR)/%.jl,%,$(wildcard $(SRCDIR)/$(group)/*.jl)))
# run `make print-TESTS` to see a list of all tests that can be run

default: all

$(TESTS):
	@cd $(SRCDIR) && \
	$(call PRINT_JULIA, $(call spawn,$(JULIA_EXECUTABLE)) --check-bounds=yes --startup-file=no ./runtests.jl $@)

perf:
	@$(MAKE) -C $(SRCDIR)/perf all

clean:
	@$(MAKE) -C perf $@

.PHONY: $(TESTS) perf clean
