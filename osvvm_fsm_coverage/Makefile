
.PHONY: sim compile clean wave

GHDL := /home/massine/ghdl/install/bin/ghdl

sim \
work/osvvm_fsm_coverage \
work/osvvm_fsm_coverage.ghw: work/osvvm_fsm_coverage.o log
	@echo Run test ...
	@cd work; $(GHDL) -r --std=08 -fpsl osvvm_fsm_coverage \
                    --psl-report=../log/osvvm_fsm_coverage.json \
                    --wave=../log/osvvm_fsm_coverage.ghw \
                    --stop-time=200ns


work/osvvm/%.o: work/osvvm
	@echo "Analyse OSVVM library ..."
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/NamePkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/TranscriptPkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/OsvvmGlobalPkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/TextUtilPkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/AlertLogPkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/RandomBasePkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/SortListPkg_int.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/RandomPkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/MessagePkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/CoveragePkg.vhd
	@cd work; $(GHDL) -a --std=08 --work=osvvm --workdir=osvvm $(OSVVM_DIR)/MemoryPkg.vhd


compile \
work/osvvm_fsm_coverage.o: osvvm_fsm_coverage.vhd work work/work work/osvvm/CoveragePkg.o
	@echo "Analyse & elaborate test ..."
	@cd work; $(GHDL) -a --std=08 -fpsl --workdir=work -Posvvm ../osvvm_fsm_coverage.vhd
	@cd work; $(GHDL) -e --std=08 -fpsl --workdir=work -Posvvm -o osvvm_fsm_coverage osvvm_fsm_coverage >& /dev/null


wave: work/osvvm_fsm_coverage.ghw
	@echo Run waveform viewer ...
	@gtkwave log/osvvm_fsm_coverage.ghw -S osvvm_fsm_coverage.tcl &


work \
work/osvvm \
work/work \
log:
	@mkdir $@


clean:
	@echo Remove generated files ...
	@rm -rf work log
