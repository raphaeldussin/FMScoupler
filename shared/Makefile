
FMS_BUILD := ../../build_fms

all: surface_flux.o surface_flux_mod.a test_surface_flux.so

surface_flux.f90: surface_flux.F90
	cpp -DuseF2PY -DINTERNAL_FILE_NML surface_flux.F90 -o surface_flux.f90

surface_flux.o: surface_flux.f90
	gfortran -c surface_flux.f90 -I$(FMS_BUILD) -L$(FMS_BUILD)/libfms.a

surface_flux_mod.a: surface_flux.o
	ar crsv surface_flux_mod.a surface_flux.o

test_surface_flux.so: surface_flux.F90 surface_flux_mod.a
	f2py -c -m test_surface_flux surface_flux.f90 -I$(FMS_BUILD) -L$(FMS_BUILD)/libfms.a -I./ -L./surface_flux_mod.a

clean:
	@rm *.a *.o *.mod *.so
