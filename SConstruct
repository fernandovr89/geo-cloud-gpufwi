from rsf.proj import *

# marmvel.hh contains Marmousi model which can be downloaded from the server using Fetch.
Fetch('marmvel.hh','marm')

Flow('vel','marmvel.hh',
        '''
        dd form=native | window j1=3 j2=3 | 
        put label1=Depth  unit1=m label2=Lateral unit2=m
        ''')
Plot('vel',
        '''
        grey color=j mean=y title="Marmousi model" scalebar=y bartype=v barlabel="V" 
        barunit="m/s" screenratio=0.45 color=j labelsz=10 titlesz=12
        ''')

Flow('shots','vel',
        '''
        sfgenshots csdgather=n fm=10 amp=1 dt=0.0015 ns=21 ng=767 nt=2800
        sxbeg=4 szbeg=2 jsx=37 jsz=0 gxbeg=0 gzbeg=3 jgx=1 jgz=0
        ''')
Result('shots','grey color=g title=shot label2= unit2=')


Plot('shot4','shots','window n3=1 f3=4| grey color=g title=shot4 label2=Lateral unit2=m')
Plot('shot11','shots','window n3=1 f3=11| grey color=g title=shot11 label2=Lateral unit2=m')
Plot('shot17','shots','window n3=1 f3=17| grey color=g title=shot17 label2=Lateral unit2=m')
Result('shotsnap','shot4 shot11 shot17','SideBySideAniso',vppen='txscale=2.')

# smoothed velocity model   
Flow('smvel','vel','smooth repeat=10 rect1=10 rect2=20')
Plot('smvel',
     '''
     grey title="Smoothed Marmousi model" wantitle=y allpos=y color=j
     pclip=100 scalebar=y bartype=v barlabel="V" barunit="m/s"
        screenratio=0.45 color=j labelsz=10 titlesz=12
     ''' )

Result('marm','vel smvel','TwoRows')

# use the over-smoothed model as initial model for FWI
Flow('vsnaps grads objs illums','smvel shots',
        '''
        sfgpufwi shots=${SOURCES[1]} grads=${TARGETS[1]} objs=${TARGETS[2]}
        illums=${TARGETS[3]} niter=30 precon=y
        ''')
Result('vsnaps',
        '''
        grey title="Updated velocity" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" 
        ''')
Plot('vsnap1','vsnaps',
        '''
        window n3=1|grey title="Updated velocity, iter=1" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')
Plot('vsnap2','vsnaps',
        '''
        window n3=1 f3=1|grey title="Updated velocity, iter=2" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')
Plot('vsnap5','vsnaps',
        '''
        window n3=1 f3=4|grey title="Updated velocity, iter=5" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')

Plot('vsnap10','vsnaps',
        '''
        window n3=1 f3=9|grey title="Updated velocity, iter=10" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')
Plot('vsnap18','vsnaps',
        '''
        window n3=1 f3=17|grey title="Updated velocity, iter=18" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')
Plot('vsnap30','vsnaps',
        '''
        window n3=1 f3=29|grey title="Updated velocity, iter=30" allpos=y color=j pclip=100 
        scalebar=y bartype=v barlabel="V" barunit="m/s" labelsz=10 titlesz=12
        ''')

Result('vsnap','vsnap1 vsnap2 vsnap5 vsnap10 vsnap18 vsnap30','TwoRows')


Result('grads','grey title="Updated gradient" scalebar=y color=j ')
Result('illums','grey title="illumination" scalebar=y color=j')

Result('objs',
        '''
        sfput n2=1 label1=Iteration unit1= unit2= label2= |
        graph title="Misfit function" dash=0 plotfat=5  grid=y yreverse=n
        ''')


End()
