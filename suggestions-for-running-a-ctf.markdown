# Introduction

This document describes some of the design decisions and technical details involved in running a CTF competition. It attempts to summarize some opinions held by the CTF community and list some specific pitfalls to avoid when designing problems. Comments or suggestsions? Pull requests welcome!

[The Many Maxims of Maximally Effective CTFs](http://captf.com/maxims.html) is another great resource with advice on how to get the most fun out of a CTF.

# General Design

It's a good idea to play plenty of CTFs so that you can know your target audience and be familiar with some of the decisions you will have to make. Remember that preparing and running a CTF always takes far more time and effort than expected, so make sure to budget plenty of time to work with :-).

## Timing

Try to use something like [ctftime.org](https://ctftime.org/) to avoid scheduling conflicts, mainly with other CTFs or security conferences. Plan ahead and announce your event early to give everyone else time to plan around it. You may further wish to look up historical dates for the larger competitions - they tend to happen around the same time every year--often tied to annual conferences--and usually can't reschedule to accommodate your CTF.

Try not to schedule your CTF during the middle of the week. This often makes playing harder for students and people with jobs.

Try to keep your competition open 24 or 48 hours: this gives teams around the world an equal amount of "daylight" hours to spend on things. It's usually a bad idea to keep things open for more than 2 days: either you can't actually fill up that much time with good problems; or you can, and competitors quickly get overwhelmed.

## Flag Format

While the flag format seems like a small detail, annoying flag formats can turn an otherwise decent CTF challenge into a frustrating experience.

Wherever possible, make the flag a simple ASCII string that competitors discover while solving the problem, and NOT a complex format like

`<md5(name of target)>_<date of event>_<function address>`

These formats are easy to accidentally under-specify! How should the name be capitalized? Does the date use slashes, dashes, or dots as separators? Does the date include the time, and in what time zone? How precise does the time need to be, seconds, milliseconds or microseconds? Is the function address in hex? Is it zero-padded to 8 or 16 digits? Does it start with "0x", or not?

Furthermore, compound flag formats often make it hard for competitors to know when they've found the right thing; there are always several permutations to try each time someone has a new guess, and it's not rewarding to fiddle with formats while wondering how close you are.

(There may be some good problems that absolutely require complex formats. Proceed with caution, and watch out for ambiguity.)

Do not ask players to hash something they get and send the hash as the flag. As with the above, it is difficult to specify exactly what needs to go into the hash, and trivial errors in capitalization or newlines will give an incorrect result. Additionally, the hash itself will not be harder to guess than whatever the original flag was, so why not just submit that directly? As an organizer, it is also useful to watch submission logs to see what people are sending in as flags to get an idea of whether or not hints are needed or players are hitting unexpected roadblocks. If all you see are a slew of MD5 hashes, you won't have a clue what is going on.

A flag should always look obviously like a flag to submit. If the flag
is "Congratulations you win!" or "1234", some competitors may not
realize when they have solved the challenge and continue to spend time
on it. It's recommended that organizers pick a common format like
"MyCTF{663d63e8c755f1b4}" or "funny\_1337speaK\_pHras3" for ALL flags in a competition. (It also helps if you can include "The flag is:" in front of some flags).

Avoid brute forcible flags. For example, if flag has to be the name of a city, some players might try to submit thousands of guesses. Please do not put a CAPTCHA in front of all key submissions. This is unnecessary and extremely annoying to players.

Be maximally permissible with flag checking. The flag submission should be case-insensitive and reasonably flexible. A nice feature is to accept any of "CTF{663d63e8c755f1b4}", "663D63E8C755F1B4", "flag is: 663D63E8C755F1B4", etc. Another nice feature is to trim/strip white space characters in flag submissions as they can easily be added when copying and pasting into web forms.

## Mechanics

CTFs are games for fun, but are also largely security skill exercises. Gameplay mechanics surrounding the challenges can add fun and intrigue, but don't always exercise security skills.  The best CTF challenges are both fun and on-topic. Having teams strategize about game mechanics isn't bad, but the core of a CTF is always its security challenges.

Therefore, any CTF scoring system--however complex--must strongly tend to reward the teams that demonstrate the best security skills. Think hard on your game mechanics, and try to keep them from being exploitable or distracting too much from the core challenges. (Not that that can't be fun, but little in this document would apply to such a competition!)

Some common mechanics for Jeopardy-style CTFs:

1. Make the more difficult "tour-de-force" problems worth more points, and the easier or higher-variance trivia and guessing problems worth fewer points. This encourages people to look at the difficult problems and learn hardcore security skillz! Don't worry about fine-tuning point values: there is no "perfect". Keep in mind that people who test a problem typically know its difficulty better than the problem author, so testers usually assign the most suitable point-values.
2. Employ "breakthru points": extra points (usually only good for tie-breaking) given to the first three-or-so teams to solve each particular challenge. This discourages "flag hoarding", where a team holds on to their flags and only submits them all at the very end of the competition (which is often advantageous for that team). It's demoralizing to find out that you are not doing as well as you thought, so it is useful to have anti-hoarding measures like breakthru points.
3. The open/closed status of a problem should be global, meaning that if a problem is open for one team, it should be open for all teams. This gives all teams a fair time and chance to look at all of the problems, and avoids situations where a team gets screwed by opening unlucky challenges.
4. Ensure that all teams on the scoreboard always have at least 2-3 problems open to work on. It's all too easy for any team to get stuck on some particular challenge (possibly through no fault of their own!), and making sure that all players have something that they'd like to work on in their hands is the best way to keep everyone happy.
5. Limit the number of unsolved challenges opened. This helps to avoid giving large teams a huge advantage. And hey, you just might be able to save some unopened challenges for next year ;)

## Testing

Testing is the difference between a horrible problem and a great one. Have a reference solution you can run to verify services are working and solvable whenever someone asks. Have someone other than the original problem author write the solution to make sure it's doable without having to be a psychic. If you don't have time to test everything, focus on the "black box" problems first.

## Communication

Organizers should strive to be reachable throughout the entire CTF. Have an IRC channel and monitor it throughout the CTF. Give organizers operator status on the channel so that players know who they can discuss problem details with.

Publish and monitor an email address.

Have at least one other communication channel like a Twitter stream or a news page on the site to ensure that nobody misses any important game updates.

## Problem Updates

Whenever a change is made to a problem, announce it in a permanent and visible place, in addition to in IRC. Make sure to update the problem description to reflect the change. For any downloadable problem files which have changed, update the file name so that it is obvious it has changed.

When applicable, leave the original version of the problem available and running. For example, someone might be really close on that pwnable you want to update and make easier :)

If a problem has already been solved, be extremely sensitive about making changes to problems (this includes changing the point value, problem files, description, hints, etc.). These issues usually need to be handled case-by-case, so please exercise good judgement!

## Problem Distribution

The easiest way to distribute files is via HTTP from your scoreboard server. Try to avoid using services like Dropbox and Google Drive, as you may hit bandwidth limits on those.

If you are super prepared and have large files, the best way to distribute them is to symmetrically encrypt them with a free tool like gpg or openssl and distribute a torrent before the CTF. You can then release the encryption passphrase at the start (or in the middle) of the CTF to give people access to the file.

Make sure that the files for problems are not accessible until the problem is opened. One simple way to do this is to append a hash of the file contents to the filename (so that filenames are not guessable) and disabling directory indexes on your webserver.

It is also nice if problems do not require logging in to download them. This allows players to easily work from the command line with tools like wget and curl, without having to set up obnoxious cookies or setup scripts to handle logging in. As long as your filenames are secret, there should be no problem with this.

## Infrastructure

Like everything else, test all parts of the infrastructure carefully. Many CTF organizers have found it useful to host their website and problems on the cloud. This makes it quick and easy to spin up more instances as needed.

Make sure to do proper testing on the final production infrastructure! For example, in the first pCTF, the pwnables were running on a machine which didn't support NX. This was missed until after the CTF because the production infrastructure was not thoroughly tested.

A partial checklist list of things to test on the production infrastructure:

* End to end testing of team registration and key submission
* Ensure key submission does not have double-counting race conditions.
* Perform load tests on scoreboard and key submission.
* Check that pwnable machines support the desired protections.
* Test full solutions for all problems - this means that you can run your solution script and it outputs the correct key. "My solution script gets eip=0x41414141" may not be enough!
* Test out the process for updating a problem during the CTF - mistakes happen, so it's best to be prepared.
* Test that the binaries/code running on your services match any files that you are giving out.

# Problems

Remember that the goal of a CTF is for the players to learn and have fun! The point of a problem is to be solved, so it's nice when every problem is solved by at least one team. Be creative and try to make sure solvers learn something cool from your problems - remember that the players are your customers, so try to make them happy :-) There are some more specific recommendations for certain categories of problems below:

## Pwnables

이번 pwnable 섹션은 일부 Linux Binary에 대한 정보만을 기술합니다.

### 로컬 문제

로컬 문제들은 보통 어떤 컴퓨터에 SSH 접속을 한 뒤  setuid/setgid 바이너리를 exploit하는 것을 목표로 합니다. 로컬 문제를 내기 위한 좋은 방법으로는 팀마다 계정을 생성하는 방법이 있습니다. 이 방법으로 팀들간에 서로 방해하거나 정보를 유출시킬 일을 없애줄 수 있습니다.다음은 로컬 문제 환경에서의 간단한 확인 사항들입니다:

* 문제 환경이 최신 버전으로 완전히 패치되었는지 확인하세요.
* limits.conf 설정을 통해 fork-bomb 사용이나 다른 자원 사용을 제한하세요
* mount -o remount,hidepid=2 /proc # 서로간 프로세스를 볼 수 없도록 설정합니다.
* chmod 1733 /tmp /var/tmp /dev/shm # 다른 사람들에게 작업물을 유출할 수 없게 해주는 겁니다.# 만약 사용자마다 홈 디렉토리가 있다면, 권한 설정을 그냥 700로 해도 됩니다 :-)
* 각 문제에 대한 사용자를 만들고, /home/problemuser/problem 형식으로 문제 바이너리를 넣어주세요.
* chown -R root:root /home/problemuser
* chown root:problemuser /home/problemuser/problem
* chmod 2755 /home/problemuser/problem
* touch /home/problemuser/flag
* chown root:problemuser /home/problemuser/flag
* chmod 440 /home/problemuser/flag


어떤 문제에 대해서든, 모두 세팅한 후에는 그 문제에 대한 모든 테스트를 수행해보세요. 특히, 당신은 아마 당신의 솔루션이 CTF 유저로서 잘 작동하는지,다른 방법으로 flag가 읽히지 않는지, 또는 root 관련 유저에 의해 쓰일 수 있는 지 확인하고 싶을 것입니다.

### 로컬 커널 문제

로컬 커널 문제는 전형적으로 어떤 환경에 SSH 접속을 한 뒤 주최측에서 만든 커널 드라이버를 exploit하는 과정입니다. 이러한 형식의 문제들은 안정적인 환경을 제공하기 힘들고, 쉽게 규모를 키우기 어렵습니다(not easily scalable, 번역 어떻게하지). 만약 exploit이 실패할 경우 전형적으로 OS가 다운되어버리기 때문에, 각 팀들은 각 팀들의 
독립된 VM이 있어야 하구요. 커널 문제들은 아마 CTF "본선"에 어울릴 지 모릅니다. 팀 수가 적고 충분한 시스템 자원이 지원되거든요.

가능한 환경으로는 팀별로 VM을 ESXi 호스트로 제공하는 것입니다. 각 팀에게 SSH 인증 정보를 주세요.

아래는 사소한 팁들과 떠올려야 할 내용들입니다:
 * 20개의 분리된 VM을 만들 바에는, 한 개의 VM을 만들고 20개의 연결된 clone(복제본)을 만드세요. (역자: vmware 등의 프로그램에 Clone이라는 기능이 있어서 그대로 번역했습니다.)
 * VM을 만드신 후 서로 구별될 수 있는 정적인 IP 주소를 구성하세요.
 * OS가 모든 공개된 취약점에 대해 패치되어있는지 확인하세요.
 * 절대 사용자에게 SUDO 권한을 주지 마세요!
 * /root 에 flag를 만드신 후 다음 명령을 실행하세요.
chmod 400 -R /root; chown root:root -R /root
 * 대회 운영진에게 SSH 키를 주고 /root/.ssh/에도 담으신 후 원격 로그인을 설정하세요. 여러가지 이슈들과 문제들을 해결하는 데 도움이 됩니다.
 * info leak 취약점이 문제의 일부가 아니라면 모든 사용자들이 /proc/kallsyms 파일을 볼 수 있게 하기 위해 다음 명령을 실행하세요: echo 0 > /proc/sys/kernel/kptr_restrict
 * 커널에서 oops가 발생해도 커널 패닉이 나지 않게 설정하세요: echo 0 > /proc/sys/kernel/panic_on_oops
 * 팀들에게 간단한(인증된) 원격 VM 리부팅 스크립트를 나누어주세요.이 스크립트는 게스트 OS가 아닌, 하이퍼바이저와 통신을 해야됩니다. 공격 시도 중 VM이 맛이 가버릴 수 있거든요. 만약 그렇게 하지 않으면 많은 팀들이 리부팅 요청을 계속 할겁니다.
 * 주어진 커널 공격 문제에 대한 잘 동작하는 해결법이 있고 그 공격법이 VM에서 제대로 동작하는지 확인하세요. 강제로 설정된 읽기 전용 메모리, SMEP/SMAP 등이 포함됩니다.


커널 문제들은 재밌을거에요! 구버전 OS를 설치해서 대회 참가자들이 공개된 exploit을 컴파일하는 일은 없게 해주세요. 당신의 문제가 창의적으로 되는 방법은 많습니다!

### 원격 문제

리모트 pwnable은 허점이 있는 네트워크 서비스가 운영되는 문제들입니다. 이 문제를 서버화하는 방법으로는 유명한 두 가지가 있는데요, xinetd를 쓰거나 바이너리 내에서 자체적으로 fork/accept를 하는 방법이 있죠.

결과에 대한 완벽한 이해 없이 스레드를 쓰지 마세요. 이런 행위는 서로간의 exploit을 방해할 수 있습니다(실수거나 고의적으로요). 또한 그 문제를 아주 짜증나게 만들 수 있어요.

만약 문제가 libc 주소 유출에 대한 문제라면, 바이너리와 함께 libc.so를 제공하는 것을 고려해보세요. libc 내의 정보(번역자: 버전 정보 없이 함수 위치를 알아낸다는 등) 알아내기 기법이 CTF에서 그다지 고려되는 기술은 아닐 수 있습니다.

If you are using your own fork/accept server instead of xinetd, you should take special care to make sure that somebody who exploits the service cannot kill or take over it. The normal way to do this is to start the service as root and drop privileges after forking (and make sure not to leak that socket fd).

See [fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c) for a sample fork/accept server following this recommendation.

See [example.xinetd](https://github.com/pwning/docs/blob/master/example.xinetd) for a sample xinetd config for an xinetd service.

If you decide to run your challenge in a chroot or restricted environment, make sure that it has basic programs like /bin/sh, /bin/bash, /bin/cat, etc. If this is not possible, then make that clear in the problem description. It is extremely frustrating to fully exploit a service and then waste an hour before realizing that it is being run in a limited chroot.

Setup instructions for a remote pwnable:

* Create a user for the problem, put the problem at /home/problemuser/problem
* chown -R root:problemuser /home/problemuser
* chmod 750 /home/problemuser
* touch /home/problemuser/flag
* chown root:problemuser /home/problemuser/flag
* chmod 440 /home/problemuser/flag

Avoid relying on short reads. These can be extremely painful to get right remotely. Instead, consider reading one byte at a time until a delimiter or reading length-delimited strings (e.g. read 4 bytes little endian length followed by length bytes of data). Similarly, make sure to check the return value on calls to read/recv to make sure you're not dropping user input.

Just to be clear, here's an example of the wrong way to read 4096 bytes:

```c
char buf[4096];
recv(fd, buf, sizeof(buf));  // This is wrong, recv might return <4096
```

For a better way, see the `recvlen` function in [fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c).

Place the flag in a predictable location such as /home/problemuser/flag. It is frustrating to waste time hunting for the flag file after you have successfully exploited a service.

### General notes

One of the most important parts of making a working pwnable is proper testing (ideally have it tested by at least one person other than the author). Any time somebody complains that a pwnable is not working, you should have a full reference solution against the live instance that you can run to verify whether the problem is working or not.

Here are some more general annoying things in pwnables:

* Obnoxious output parsing. Please keep the output simple and reasonable to parse. The best type of output to parse is a length delimited string.

  Example of an obnoxious output format:
  ASCII decimal length delimited strings: 121A1B1C1D1E1F
  How are we supposed to parse this? Is it (12, '1A1B1C1D1E1F') or it is (1, '2'), (1, 'A'), (1, 'B'), (1, 'C'), (1, 'D'), (1, 'E'), (1, 'F')?

  Another obnoxious output format is stuff with ANSI escape codes. Have some self control, please :-)
* Even though the binary shows that NX is enabled, the machine it's running on doesn't support it.
* Nonsensical code and "fake" bugs. If 90% of your code is just checking inputs against a bunch of random constants to waste the reverser's time, it is probably not a very fun problem. If your bug is "the program jumps into this buffer for no good reason when these random constraints are satisfied" then you should probably push yourself to be a little more creative :-)

### Compile time protections

Often, a pwnable requires a specific set of protections to be enabled. Here's how to force them on / off in `gcc`

* `-fstack-protector` / `-fno-stack-protector`: Stack canaries
* `-D_FORTIFY_SOURCE=2` / `-D_FORTIFY_SOURCE=0` (prepend `-U_FORTIFY_SOURCE` to silence re-definition warnings): Use "`*_check`" versions of libc functions like `memcpy()`, `sprintf()`, `read()`, etc. that abort when they detect buffer overflows. (Note that detection is far from perfect and does not work in many cases.)
* `-fPIE -pie` / `-fno-PIE`: Position independent code (extends ASLR to also randomize the main binary, not just libraries). Note that PIE is usually ineffective on 32 bit, i.e. a PIE-unaware exploit will land once every couple hundred/thousand times. `-fPIC` is a version of `-fPIE` that doesn't require the resulting code to be part of the main executable (by avoiding certain optimizations), and there's also `-fpie` and `-fpic` which are dumb.
* `-Wl,-z,relro,-z,now` / `?`: Full RELRO (the GOT and PLT will be written and mapped read-only during program load).

As security becomes a more mainstream issue (yay!), more compile-time and runtime protections are being enabled by default. So, to avoid surprises you should really test your problems in their final configuration & setup.

## Web Challenges

If your challenge requires a large number of requests or measuring timing, ensure that it is still reasonable to solve remotely. Better yet, have a shell server on the same network (perhaps a pwnable with an SSH login) with common scripting languages and libraries installed where players can run their attacks from.

Here are some common things to avoid:

* Requiring players to guess URL parameters out of thin air (like ?debug=1)
* Requiring players to guess file or directory names (or dos your server with dirbuster)
* Requiring players to guess credentials

(Notice a pattern here?)

One great type of web problem is one where full source is given (and despite this it is still challenging).

## Reversing

It is very important to ensure that problems which check whether an input is valid always accept exactly one solution. This is by far the most common problem-breaking mistake in reversing challenges. If this is not possible to do, then make this clear in the problem description and have a form which accepts any input flag and outputs a flag that can be submitted for points.

Although a common real world use of reverse engineering is malware, it is generally considered to be in bad taste to distribute malicious reverse engineering challenges. If you are going to do this, be sure to clearly state that the programs are malicious. 

## Crypto

In general try to give the competitors as much information as you can. *Working* source code with secret keys clearly X'ed out is ideal.

If you have a ciphertext only problem, try to do the following:

* Give enough ciphertext for meaningful statistics (twenty ASCII characters can be almost anything)
* Use a guessable algorithm. With a classic cipher and a short amount of ciphertext, it may be very difficult to narrow it down. The challenge should be in breaking the crypto system, not figuring out what the crypto system is.
* If you are using any crypto system made after the 19th century, be sure to tell challengers what the algorithm is. No one wants to guess whether ciphertext is from an Enigma or a Purple Machine; or from 3DES or GHOST

If your problem requires a lot of local computation, make sure to test it on reasonable consumer hardware. Realizing late in the competition that while you have the right method of solving the probem, you don't have the computation power to complete the computation before the end is a bad feeling. In general, under an hour on fairly modern consumer hardware is fine; more than that should have good justification.

## Forensics

## Miscellaneous

Try to avoid these as much as possible:

* Random guessing challenges
* Cracking passwords on zip files or stego programs
* Steganography problems
* Anything that is solved by just running metasploit, nessus, dirbuster, etc. Good CTF problems should require skill.
* Time-consuming recon challenges.
