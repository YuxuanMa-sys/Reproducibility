BornholtT18.yaml BuildAndTestSuccess
CaiGZACOTTW18.yaml BuildAndTestSuccess
WeiZZCMLFC23.yaml BuildAndTestSuccess
FengKRR12.yaml BuildAndTestSuccess
YangLW12.yaml BuildAndTestSuccess
BegoliCHML18.yaml BuildAndTestSuccess
GehrMTVWV18.yaml BuildAndTestSuccess
ShahKZ17.yaml BuildAndTestSuccess
LimFAK11.yaml BuildAndTestSuccess
# 0001ST18.yaml, KangKSLPSKCCHY18.yaml, PulteFDFSS18.yaml: Going to experiment with base image next week
KaiserZKRD18.yaml BuildCrash
LeeHJLRL18.yaml BuildIncomplete
MbakoyiannisTK18.yaml BuildIncomplete
PrakriyaCBSC24.yaml BuildCrash
JinLJMSHZ18.yaml BuildCrash

----

dockerfile successful but dockerfile exported by the validator unsuccessful (all the successful dockerfile are under
corresponding directories):

FengKRR12.yaml ( runc run failed: unable to start container process:cexec: "/bin/sh": stat /bin/sh: no such file or
directory)

> The only significant difference between the YAML and the pure Dockerfile is that the pure Dockerfile sets the current working directory (`WORKDIR`) while the YAML does not (so it is the default, `/`). Then  calling tar --strip 1 will extract-and-overwrite. If the archive contained a bismarck/bin/..., --strip 1 takes off the prefix, and it overwrites the containers `/bin` directory (since cwd == `/`), and that explains why "`/bin/sh`: no such file". Inserting `cd` before untarring fixes this problem.
>
> Also note that the test in both the Dockerfile and in the YAML are insufficient. We should execute the tests at _build time_ (`RUN tests`), not run-time (`CMD tests`). The rubric dictates I have to take off 3 points, but you can earn them back next week. MP2 explains how to test servers at build time (basically, `RUN start server && wait for server ready && send request && check response && shutdown server`).
> --Sam

BegoliCHML18.yaml (./gradlew: not found) , there is another ./gradlew command which is successful

> I haven't tested it, but this seems like it simply needs `cd`.

GehrMTVWV18.yaml (>.bayonet: no such file)

> The problem here is `>` needs to be encoded using `redirect_stdout`. I only want to support a subset of shell features, to make the script more analyzable without parsing arbitrary shell syntax; it's essentially the same difference between `subprocess.run(...)` and `subprocess.run(..., shell=True)` in Python.
> --Sam

ShahKZ17.yaml (gpg: keyserver receive failed: Connection timed out)

> I twiddled that file around a bit, and I got it to pass this step and fail on a later one. --Sam

Failed to fill with validator schema:

0001ST18.yaml, KangKSLPSKCCHY18.yaml, PulteFDFSS18.yaml: Not using ubuntu base image and need sudo

> From our discussion over Canvas, I am still unsure whether using a different base image is necessary. If it ends up being necessary that is fine, but please:
>
> 1. cd KaiserZKRD18 && docker build
> 2. If that works, perhaps the same strategy will work for the three you have here. If it does not work, please send me the error. Does it at least get past the point you used to be stuck at when you tried using an Ubuntu base image? I couldn't tell.
> 3. I may have a follow-up suggestion, in which case go back to 2. Otherwise, we can declare it as "necessary".
>
> --Sam

LimFAK11 googletest and the project need different version of gcc, I used 'add-apt-repository ppa:
ubuntu-toolchain-r/test' in my dockerfile and it's not accepted in the schema

> I believe the problem there is actually the `apt-get clean`. The validator should only complain about `apt` and `apt-get`. --Sam

Failed: KaiserZKRD18.yaml, LeeHJLRL18.yaml, MbakoyiannisTK18.yaml, PrakriyaCBSC24.yaml, JinLJMSHZ18.yaml

> By the way, I added `DEBIAN_FRONTEND` the dockerfile-generator in 0.2.13, so please remove it from your YAML files. When the frontend is noninteractive, the system does not prompt for a timezone, which will default to UTC. You may write a timezone explicitly if you want for debugging, but please remove it before committing, unless it is strictly necessary.
>
> --Sam

> I acknowledge that my code for YAML validation/dockerfile-exporting is quite error-prone and buggy. You've done a great job of adapting to my buggy system. It would be a good challenge to try report the root cause (or one level closer to it) of the errors, rather than noting that it doesn't work. For example, the `add-apt-repository` is fine; it was actually `apt-get clean`. Sometimes the current working directory may be the cause. Trying to "dig" one level deeper will be a good exercise. In particular, bisecting a working and broken version can be a useful strategy in other software engineering problems. Also don't hesitate to reach out on Canvas if you encounter difficulties. --Sam

> I haven't gone through all the penalties for progress4 yet, but if you don't get too many penalties, your score will be about 105 / 40.

# Complete response

# BornholtT18

I renamed `Bornholt18.yaml` to `BornholtT18.yaml` to match the URL.

# PrakriyaCBSC24

I got a bit further on this one, but still hit an unsolvable compile error.


## After regrading
yuxuanm4: BegoliCHML18.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: BornholtT18.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: CaiGZACOTTW18.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: FengKRR12.yaml: 5.0 (TestFail)
yuxuanm4: GehrMTVWV18.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: JinLJMSHZ18.yaml: 5.0 (BuildCrash)
yuxuanm4: KaiserZKRD18.yaml: 5.0 (BuildCrash)
yuxuanm4: LeeHJLRL18.yaml: 5.0 (BuildIncomplete)
yuxuanm4: LimFAK11.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: MbakoyiannisTK18.yaml: 5.0 (BuildIncomplete)
yuxuanm4: PrakriyaCBSC24.yaml: 5.0 (BuildCrash)
yuxuanm4: ShahKZ17.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: WeiZZCMLFC23.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: YangLW12.yaml: 10.0 (BuildAndTestSuccess)
yuxuanm4: total 110.0