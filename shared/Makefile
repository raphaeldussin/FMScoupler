
FMS_BUILD := ../../build_fms

all: offline_surface_flux.so

surface_flux.f90: surface_flux.F90
	cpp -DuseF2PY -DINTERNAL_FILE_NML surface_flux.F90 -o surface_flux.f90

offline_surface_flux.so: surface_flux.F90
	f2py -c -m offline_surface_flux surface_flux.f90 -I$(FMS_BUILD) -L$(FMS_BUILD) -lfms

clean:
	@rm *.a *.o *.mod *.so
