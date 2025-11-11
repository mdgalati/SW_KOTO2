# Getting Started

### 📖 Manual
A manual is available here: [gsim4.pdf](https://github.com/mdgalati/SW_KOTO2/blob/main/gsim4/gsim4.pdf)

---

###  Main Code Location
The main simulation code can be found at:
 ```/sw/koto2/e14lib/201605v6.5/e14/examples/gsim4test/gsim4test.cc```

### Environment Setup
Before running the code, load the appropriate environment:
```source /sw/koto2/e14lib/201605v6.5/env.sh```
It might be convenient to add this to your ```~/.bashrc```

---

### Requirements
A macro file that defines the detector geometry, i.e. [step2geom15cm.mac](https://github.com/mdgalati/SW_KOTO2/blob/main/gsim4/step2geom15cm.mac)

A macro defining beam and decay mode, i.e. [step2geom_step2mome15cm_pi0ee.mac](https://github.com/mdgalati/SW_KOTO2/blob/main/gsim4/step2geom_step2mome15cm_pi0ee.mac)

### Running the simulation
```gsim4test <your_macro>.mac <output_file>.root <#events> 0 0```

For example: ```gsim4test step2geom_step2mome15cm_pi0ee.mac KLpi0ee.root 1000 0 0```
