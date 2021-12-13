A LAMMPS input script typically has 4 parts:

Initialization
System definition
Simulation settings
Run a simulation


# Initialization
Usually the relevant commands are 
1. units
    
    Syntax : units style
             style = lj | real | metal | si | cgs | electron | micro | nano

2. dimension

    Syntax : dimension N
             N = 2 or 3

3. newton

    Syntax : newton flag
             
             newton flag1 flag2

             flag = on or off for both pairwise and bonded interactions
             flag1 = on or off for pairwise interactions
             flag2 = on or off for bonded interactions
             Examples: 
                    newton off
                    newton on off

4. processors

    Syntax : processors Px Py Pz keyword args ...

             Px,Py,Pz = # of processors in each dimension of 3d grid overlaying the simulation domain
             zero or more keyword/arg pairs may be appended
             keyword = grid or map or part or file

5. boundary

    Syntax : boundary x y z

            x,y,z = p or s or f or m, one or two letters

            p is periodic
            f is non-periodic and fixed
            s is non-periodic and shrink-wrapped
            m is non-periodic and shrink-wrapped with a minimum value

        Examples : 
                boundary p p f
                boundary p fs p
                boundary s f fm

6. atom_style

    Syntax : atom_style style args

            style = angle or atomic or body or bond or charge or dipole or dpd or edpd or electron or ellipsoid or full or line or mdpd or molecular or oxdna or peri or smd or sph or sphere or spin or tdpd or tri or template or hybrid

                    args = none for any style except the following
                    body args = bstyle bstyle-args
                        bstyle = style of body particles
                        bstyle-args = additional arguments specific to the bstyle
                                   
                    sphere arg = 0/1 (optional) for static/dynamic particle radii
                    tdpd arg = Nspecies
                        Nspecies = # of chemical species
                    template arg = template-ID
                        template-ID = ID of molecule template specified in a separate molecule command
                    hybrid args = list of one or more sub-styles, each with their args
                    accelerated styles (with same args) = angle/kk or atomic/kk or bond/kk or charge/kk or full/kk or molecular/kk or spin/kk


            angle   |	bonds and angles    |	bead-spring polymers with stiffness
            ---------------------------------------------------------------------
            atomic  |	only the default values |	coarse-grain liquids, solids, metals
            ----------------------------------------------------------------------
            body    |	mass, inertia moments, quaternion, angular momentum |	arbitrary bodies
            bond    |	bonds   |	bead-spring polymers
            charge  |	charge  |	atomic system with charges
            dielectric  |   dipole, area, curvature |	system with surface polarization
            dipole  |	charge and dipole moment    |	system with dipolar particles
            dpd |	internal temperature and internal energies  |	DPD particles
            edpd	temperature and heat capacity	eDPD particles
            electron	charge and spin and eradius	electronic force field
            ellipsoid	shape, quaternion, angular momentum	aspherical particles
            full	molecular + charge	bio-molecules
            line	end points, angular velocity	rigid bodies
            mdpd	density	mDPD particles
            mesont	mass, radius, length, buckling, connections, tube id	mesoscopic nanotubes
            molecular	bonds, angles, dihedrals, impropers	uncharged molecules
            oxdna	nucleotide polarity	coarse-grained DNA and RNA models
            peri	mass, volume	mesoscopic Peridynamic models
            smd	volume, kernel diameter, contact radius, mass	solid and fluid SPH particles
            sph	rho, esph, cv	SPH particles
            sphere	diameter, mass, angular velocity	granular models
            spin	magnetic moment	system with magnetic particles
            tdpd	chemical concentration	tDPD particles
            template	template index, template atom	small molecules with fixed topology
            tri	corner points, angular momentum	rigid bodies
            wavepacket	charge, spin, eradius, etag, cs_re, cs_im	AWPMD


7. atom_modify

    Syntax : atom_modify keyword values ...
            one or more keyword/value pairs may be appended

            keyword = id or map or first or sort

                        id value = yes or no
                        map value = yes or array or hash
                        first value = group-ID = group whose atoms will appear first in internal atom lists
                        sort values = Nfreq binsize
                        Nfreq = sort atoms spatially every this many time steps
                        binsize = bin size for spatial sorting (distance units)
                        Examples
                        atom_modify map yes
                        atom_modify map hash sort 10000 2.0
                        atom_modify first colloid



If force-field parameters appear in the files that will be read, these commands tell LAMMPS what kinds of force fields are being used: pair_style, bond_style, angle_style, dihedral_style, improper_style.