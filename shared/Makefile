
FMS_BUILD := build
MKMF_ARCH := linux-ubuntu-xenial-gnu.mk

all: build/libfms.a offline_surface_flux.so

FMS mkmf:
	git clone https://github.com/NOAA-GFDL/$@

build/libfms.a: FMS mkmf
	mkdir -p build
	(cd build/; rm -f path_names;)
	(cd build/; ../mkmf/bin/list_paths -l ../FMS/include ../FMS/fms ../FMS/fms2_io ../FMS/monin_obukhov ../FMS/sat_vapor_pres ../FMS/constants ../FMS/mpp ../FMS/platform ../FMS/memutils;)
	(cd build/; ../mkmf/bin/mkmf -t ../mkmf/templates/$(MKMF_ARCH) -p libfms.a -c "-Duse_libMPI -Duse_netCDF -DSPMD -fPIC" path_names)
	(cd build/; make NETCDF=3 REPRO=1 libfms.a -j)

surface_flux.f90: surface_flux.F90
	cpp -DuseF2PY -DINTERNAL_FILE_NML surface_flux.F90 -o surface_flux.f90

offline_surface_flux.so: surface_flux.f90
	f2py -c -m offline_surface_flux surface_flux.f90 -I$(FMS_BUILD) -L$(FMS_BUILD) -lfms

clean:
	@rm *.a *.o *.mod *.so surface_flux.f90

distclean:
	@rm -rf FMS mkmf build
