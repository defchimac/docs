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

만약 당신이 xinetd 대신에 자체적인 fork/accept를 쓰고 있다면,그 서비스를 공격하는 사용자들이 그 서비스를 종료시키거나 다운시킬 수 없게 하세요. 이를 수행하는 일반적인 방법은 root 권한으로 서비스를 돌리고 fork 후에 권한을 떨어뜨리는 방법입니다(그리고 그 소켓 fd에 대한 정보를 유출시키지 않는 것입니다).

[fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c) 에는 언급한 권장사항들을 따르는 fork/accept 서버 예제 코드가 담겨있습니다.

[example.xinetd](https://github.com/pwning/docs/blob/master/example.xinetd) 에는 xinetd 서비스에 대한 예제 설정 파일이 있습니다.

당신이 chroot 환경이나 제한된 환경 안에서 문제들을 돌릴 것이라면, 기본적인 프로그램인 /bin/sh, /bin/bash, /bin/cat 등은 있는 지 확인해보세요. 만약 없어야 한다면,문제 설명에 분명하게 써놓으세요. 어떤 서비스를 다 뚫어놓고 그것이 제한된 chroot 환경이란 것을 알기 위해 몇 시간을 소모하는 것은 극도로 짜증날 수 있습니다.

원격 pwnable 문제에 대한 설정을 단계별로 서술해놨습니다:

* 그 문제에 대한 사용자를 만들고, 다음 위치에 문제를 넣어두세요: /home/problemuser/problem
* chown -R root:problemuser /home/problemuser
* chmod 750 /home/problemuser
* touch /home/problemuser/flag
* chown root:problemuser /home/problemuser/flag
* chmod 440 /home/problemuser/flag

짧은 길이에 대한 read 함수에 의존하지 마세요. 이는 exploit을 원격 환경에 맞추는 것을 극도로 어렵게 합니다. 대신 어떤 구분자를 받을 때까지 1바이트씩 읽어오거나, 길이를 지정하면서 동일한 방식으로 read를 수행하세요 (예시:4바이트의 리틀 엔디안 정수로 길이를 수신하고, 그 길이만큼 read를 수행). 비슷한 경우로, read와 recv의 리턴 값을 체크하여 클라이언트로부터 데이터를 제대로 수신하고 있는지 확인하세요.

명확하게 하기 위해, 4096바이트를 받아오는 잘못된 예시를 보여드리겠습니다:

```c
char buf[4096];
recv(fd, buf, sizeof(buf));  // This is wrong, recv might return <4096
```

더 좋은 방법으로는, [fork_accept.c](https://github.com/pwning/docs/blob/master/fork_accept.c) 안에 있는 `recvlen` 함수를 참조하세요.

flag 파일을 /home/problemuser/flag 와 같은 예상할 수 있는 경로로 놓아주세요. 서비스를 성공적으로 익스플로잇 하고 난 뒤 플래그를 사냥하는 일은 극도로 짜증납니다.

### 일반적인 팁

작동하는 pwnable 문제를 만들기 위한 일반적인 팁은 “테스트”입니다 (적어도 문제 제출자 외에 한 사람은 더 테스트해봐야 합니다).누군가가 pwanble 문제가 제대로 작동하지 않는다고 민원을 제기할 때,그 문제가 제대로 돌아가고 있는지 현장에서 바로 확인할 수 있는 완전한 방법이 있어야 합니다.

아래 사항들은 pwnable 문제들 중 일반적으로 짜증나는 부분들입니다:

* 불쾌한 출력문 파싱.출력을 간단하고 파싱하기 쉽게 만들어 놓으세요. 파싱하기 가장 좋은 출력의 유형은 길이가 일정한 문자열입니다.
 불쾌한 출력문의 예시입니다:
 ASCII 문자열 + 숫자로 이루어진 길이가 일정한 문자열: 121A1B1C1D1E1F 이런 건 어떻게 파싱해야 할까요? (12, '1A1B1C1D1E1F') 인가요, (1, '2'), (1, 'A'), (1, 'B'), (1, 'C'), (1, 'D'), (1, 'E'), (1, 'F')?
 또 다른 예로는 ANSI escape 문자열입니다. 이런 건 자중해주세요, 좀 :-)
* 바이너리 상으로는 NX가 켜져있는데, 바이너리가 실행되는 환경에서는 NX를 지원하지 않는 경우가 있습니다.
* 황당한 코드들과 "가짜" 버그들. 만약 90%의 코드가 입력값과 랜덤값을 비교하는 코드들이라면, 문제가 그리 재밌진 않을 것입니다. 만약 프로그램의 버그가 "어떤 랜덤한 조건이 성립되었을 때 프로그램 흐름을 버퍼로 조작할 수 있는 버그"라면 아마 조금 더 창의적인 시도를 할 수도 있을 것입니다:-)


### 컴파일러 옵션을 통한 보호 기법

pwnable 문제들은, 자주 일련의 보호 기법이 사용되어야 될 때가 있습니다. 아래의 옵션들은 `gcc`에서 그 옵션들을 키거나/끄는 방법입니다.

* `-fstack-protector` / `-fno-stack-protector`: 스택 카나리
* `-D_FORTIFY_SOURCE=2` / `-D_FORTIFY_SOURCE=0` (`-U_FORTIFY_SOURCE` 를 앞에 넣으면 재선언 관련 경고들을 줄일 수 있습니다): `memcpy()`, `sprintf()`, `read()` 등등의 함수들은 libc의 "`*_check`" 버전을 쓰세요. 이 함수들은 버퍼 오버플로우가 감지되면 프로그램을 중단시킵니다. (여기서 쓰인 방식이 완벽하진 않고 여러 상황에서 잘 작동하지 않는 점을 명심해주세요)
* `-fPIE -pie` / `-fno-PIE`: Position independent code, 즉 PIE라고 불리는 바이너리를 컴파일하는 옵션입니다 (ASLR을 라이브러리가 아닌, 실행 파일까지 확장시켜줍니다). PIE 는 32 bit 운영체제에서는 그리 효과가 없다는 점을 알아두세요, 가령 PIE를 신경쓰지 않은 익스플로잇이 한 주소로 수백, 수천번 실행될 수 있습니다. `-fPIC` 옵션은 `-fPIE` 옵션에서 결과 코드가 꼭 메인 바이너리에 포함되지는 않게 해줍니다 (그와 관련된 일련의 최적화를 방지합니다), 그리고 기능이 동일한 `-fpie`, `-fpic`  옵션도 있습니다.
* `-Wl,-z,relro,-z,now` / `?`: Full RELRO 옵션입니다 (GOT랑 PLT가 프로그램 로딩 시 미리 기록되고 읽기 권한 메모리로 매핑됩니다).

보안이 메인 이슈가 되는 만큼 (예!),기본적으로 많은 컴파일 시간대의 보호 기법과 런타임 보호 기법이 기본 옵션으로 적용되고 있습니다. 그래서, 당황할만한 상황을 미연에 방지하기 위해서 마지막 설정 및 설치 후 꼭 확인을 다시 해봐야 할 것입니다.

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
