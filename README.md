
⚠️ I do not own any right on the images here displayed, if the owners is not ok with me using them, please notify me and I'll remove them. Anyway they are planned to be removed soon in a upcoming commit. ⚠️ 



![Alt text](./scaled-logo-uniroma3.jpg "a title")
![Alt text](./department.jpg "a title")

# **IN490** Final Assignment V0.0.1
### Title: _Views on "RustyHermit - A Rust-based, lightweight unikernel"_
### Author: _Alessio Proietti <<alessio.proietti@stud.uniroma3.it>>_

</br>

## Virtualization 
In the long gone days of early July 1974, Juan Perón, an Argentine army general, politician and President of Argentina died after getting into difficulty following a heart attack. In the same days, thousands of miles away of Argentina, American computer scientists were unlocking the potential of optimal usage of the then called third generation computer systems, e.g. IBM 360/67.

![Alt text](./IBM36067.jpg "IBM System/360 Model 67 (S/360-67)")

Then a foundational paper of Gerald J. Popek and Robert P. Goldberg finally  appeared on the 17th volume of _Communications of the ACM_.

Its title reads _Formal requirements for virtualizable third generation architectures_ and in the body of the text were posited a set of requirements for a computer architecture to support a legit form of system virtualization.

Virtualization amounts to creation of a virtual machine, by means of both hardware and software layers that can virtualize computing resources, such as processors, memory, storage and network connectivity as well as emulating required peripheral devices.

For a virtualization platform to qualify as such it is mandatory to present at least the following features: 

- _Fidelity_ : A program running under a virtual machine monitor (VMM) and inside a virtual machine should run near exactly as it would if executed on a real machine. 
- That is to say the program could expect to be _kicked back_ as intended if it _kicks_ ( = makes) requests to its emulated environment. **VMs perfectly render computing resources**. 

- _Safety_ : Nothing from inside virtual machines should escapes the grasp and the sight of the VMM. VMM has full secure control so it can ensure protection of data and resources. In addition, VMM prevents interference between emulation contexts. **Virtual machines play in a sandbox arranged by VMM**.

- _Efficiency_ : VMM mediation and monitoring inevitably degrade performance. Most of execution of privileged instructions must be handled as lightly as possible with hardware or software support and not by the VMM itself. **Efficient Hypervisor (a.k.a. VMM) must delegate**. 

![Alt text](./panopticon.jpg "Jeremy Bentham's Panopticon")

## VMs and Containers
Virtual machines have been a great deal in the old mainframe world and in the early days of the World Wide Web and the commercial Internet too after widespreading demand of hosting services, also today virtualization importance in exploding _cloud computing_ industry cannot be overstated.

More recently a new paradigm walked into the stage as a promising alternative theory of workload, enabled by so called Operating System-level virtualization (Shared Kernel virtualization). Virtual machines are bulky and need maintenance, proper configuration of software and serious management of software updates. In principle virtual machines could serve as general purpose host but rapidly tend to become high specialized. 

However also highly specialized VMs usually carry tons of unused, opaque and potentially flawed software, exposing large attack surface. It is a flexibility and security nightmare. 

![Alt text](./containers.jpg "Apples-to-Oranges comparison")

Luckily, improvements and research in computer engineering and in particular in BSD and Linux Kernel development stream continuously. At the turning of the century engineers and computer scientists came up with the idea of *chroot jails*, *cgroups* and later *containers*. 

Jails were first implemented in FreeBSD as a consequence of the desire to establish clean compartmentation between services competing in a shared environment, tipically small, while confining the so called "omnipotent root" (Poul-Henning Kam).

With jails and subsequent refining of the concept (aka containers) it is now possible to multiplex _userspace_ and implement _application-level_ segregation of environments for execution. 

Proponents and adopters of this relatively new architecture of computing are trying to fullfill the vision of VMs as _generic hosts_ for _multiple_, _indipendent_ and _specialized_ userspaces called _containers_ that should be as small as a single application and easy to manage, create, start, stop, destroy and connect together. **Crucially containers are supposed to be stateless and light.** 

Being stateless they can be succintly described and arranged with simple DSLs, being light they offer little surface of attacks to bad actors.

## Unikernels
> "Bloat Is a Bigger Issue Than You Might Think." 
>  
> Russel C. Pavlicek
 
Cloud is fat. VMs are overloaded with unpatched, obscure and insecure software and Containers themselves rely on huge kernels, i.e. on large supporting runtimes. 
Moreover, developers usually make every nasty trick they can to wire correctly all the resources the containers need.

The theory of Unikernels introduces a pretty simple and straightforward strategy. Unikernels are introduced as a new kind of tech which try to coexist with VMs and cointainers while aiming at resolving their deficiencies.  

The rationale can be clearly stated: one should include into sandbox objects (containers, jails, VMs) only minimum code application needs to properly function

Unikernels are **Specialized Kernels**.

![Alt text](./road_to_unikernels.png "Apples-to-Oranges comparison")

Implementations of Unikernel technology have only one address space, only one program running inside and all low-level functions compiled directly into the unikernel-app executable and that's why advocates of Unikernels call them _Library Operating Systems_. 

All boils down to a binary with app and support libraries and nothing else. It feels like a backward evolution but it is not. 

Unikernels have less attack surfaces, small footprints and they can be tailored on the precise app they hosts and crucially they boot faster. In a world of transient microservices which pop into existence after response triggered by events happening at random by ( see _Internet of things_ ) seems like Unikernels are a must.

It is important to highlight how Unikernels have no shell, no alien utilities to exploit, unused drivers or password files to crack and no databases to breach. Though special care is required while debugging Unikernels it can be done efficiently and correctly.

## Rust
Rust is a "multi-paradigm, general-purpose programming language designed for performance and safety, especially safe concurrency" (Wikipedia). Best source of knowledge for Rust is for sure its home on the web, https://www.rust-lang.org/. 

![Alt text](./rust-logo-blk.svg "Apples-to-Oranges comparison") 

Rust comes in various "flavours": nightly, beta, stable. One should install the _Rust Toolchain_ with the official installer https://rustup.rs/ because one expects to have the possibility to enrich the programming environment easily with additional packages and "components" in order to perform very peculiar compilation jobs (like in low-level programming tasks e.g. write bare-metal applications and kernels).
  

## A "Close Encounter" with RustyHermit ![Alt text](./hermitcore_logo.png "Apples-to-Oranges comparison")

![Alt text](./close-encounters-bluray.jpg "Apples-to-Oranges comparison") 

RustyHermit (https://github.com/hermitcore/rusty-hermit) is an attempt to port and modernize _HermitCore's C_  codebase (http://hermitcore.org/) to _Rust_. 
It is developed by a small internet community lead by researchers at at RWTH-Aachen. The project aims at establish a robust Unikernel candidate for HPC workloads.

RustyHermit has a couple of companion projects. 
One is _uhyve_ (https://github.com/hermitcore/uhyve) that is a custom hypervisor which requires both KVM and a system supporting virtualization and is tailored for RustyHermit-based unikernels, the other is a custom light bootloader (https://github.com/hermitcore/rusty-loader) which is needed when RustyHermit-based unikernels are run with QEMU. 

*Disclaimer*: The following amounts to a mere description of the _author's personal experience_ with this object.

There has been a first round of exploration of RustyHermit potential following step by step the official documentation of the project. Unfortunately fetching RustyHermit's crate from Cargo Repository and using it as a dependency in a blank project resulted in repeatedly unsuccesful compilation attempts. Each iteration of the build project introduces new displayed errors and quite unclear messages from the prompts. 

There have been ebbs and flow of succesful compilations with then unusable "hello world" unikernels or inexplicably failed compilations. History of this unfruitful quest is documented by the author's (@alessio-proietti) tickets on the official Issue Tracker of the project (https://github.com/hermitcore/rusty-hermit/issues).

Later on, a second phase of the research started with the author initially cloning the original and complete repository of RustyHermit and then compiling the application code of the demo shipped with the repo. 
This choice was forced after noting that configuration code and building options was present in the repository and the demo apps in fact relied heavily on this fine-tuned _corpus_ of code to work. 

_If it works, don't break it._

## A Second Attempt

More recently the team behind rusty-hermit released a template demo project on which it is possible to iterate towards custom and arbirtary complicated kernels. 
The author tried forcing the connectivity between various instances of rusty-hermit, he failed again. 

It is worth noting that the vanilla demo (and projects which don't need network) at least compiles and runs. You can find more information in details here https://github.com/hermitcore/rusty-demo.

I'll copy here some steps you should do to make a demo up and running.

1) Be sure to have rustup, QEMU installed as well as NASM assembler which is required to SMP in x86_64 architecture.

2) Clone https://github.com/hermitcore/rusty-demo with `git` and initialize all the submodules, for references https://git-scm.com/book/en/v2/Git-Tools-Submodules.

3) Build the bootloader:
   ``` 
   $ cd loader
   $ cargo xtask build --arch x86_64 --release
   ```
1) Build the kernel: 
   ```
   $ cargo build \
   -Zbuild-std=core,alloc,std,panic_abort \
   -Zbuild-std-features=compiler-builtins-mem \
   --target x86_64-unknown-hermit \
   --release
   ```

2) Finally run with QEMU:
   ```
   $ qemu-system-x86_64 \
    -cpu qemu64,apic,fsgsbase,fxsr,rdrand,rdtscp,xsave,xsaveopt \
    -smp 1 -m 64M \
    -device isa-debug-exit,iobase=0xf4,iosize=0x04 \
    -display none -serial stdio \
    -kernel loader/target/x86_64/release/rusty-loader \
    -initrd target/x86_64-unknown-hermit/release/hello_world
   ```

## Playing around with a small non-trivial demo
### Envelope Encryption

Envelope Encryption is a a cryptographic scheme in which a TTP hosts a hardened keyring with a master key (CMK) inside. What problem is Envelope Encryption supposed to solve? A client wants to do encryption and decryption of data but doesn't want to persist a secret data key. 

The procol tries to satisfy request for encryption without storing keys.

It goes like this: The Server (the TTP) generate a fresh data key on behalf of the client and send it together with an encrypted copy over a secure authenticated channel. 
The client encrypts its data and makes a tuple out of data and corresponding encrypted data key and discard(!) the plaintext data key. The clients stores the tuple. 

Later on, when client needs to decrypt data it calls the Server and sends over a secure authenticad channel the encrypted key, the Server responds sending back the plaintext version. Client now decrypts then discard the plaintext version of the secret data key. 

I will not discuss how flawed can be Envelope encryption, I want only to let you know I assembled a small demo built on rusty-demo importing the Rust `dryoc` cryptographic crate, it worked. 

In the future it can be made possible to have a networked Envelope Encryption server up and running based on rusty hermit Unikernel. Such a development could be interesting because Unikernels certainly fit for a transient complex object which does heavy encryption/decryption efficiently and safely before fading away.

Here the author's take on Envelope Encryption based on rusty-demo: https://github.com/alessio-proietti/envelope-encryption-rusty-demo.

For references, here a blog post to review how Envelope Encryption can be built on top of AWS KMS https://enlear.academy/envelope-encryption-in-aws-d1a03eeed7c.

## Conclusions

RustyHermit is definitely too young and quite unstable framework for production yet it is promising. It should be noted that Unikernel working PoC would be welcomed in the HPC space.

A final remark, Unikernels are not a _Panacea_ and nothing is. They are not bound to completely outperform Containers and the best outcome of Unikernels research is, perhaps, a contamination and a (eventually) merge of Containers and Unikernels best intuitions: _Less is more_.

![Alt text](./thats-all-folks.jpg "Apples-to-Oranges comparison")


## References
- G. J. Popek and R. P. Goldberg. Formal Requirements for virtualizable third generations architectures. _Comms. ACM_, 17(7):412-421, July 1974.
- Pavlicek, Russell. Unikernels. _O'Reilly Media, Inc._, 2016.
- Kamp, Poul-Henning; N. M. Watson, Robert (2000). "Jails: Confining the omnipotent root" (PDF). PHKs Bikeshed. 