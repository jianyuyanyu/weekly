---
date: 2026/08/31
---

<img src="https://cdn.tw93.fun/uPic/28049.jpg" width="800" />

<small>The cover photo was taken at the newly opened Hang Lung mall in Hangzhou, where I wandered around over the weekend. I had a cup of AITCoffee, which tasted pretty good, and spotted this paper window on the wall with the character for Song on it. I have been using this typeface on my product sites lately, and it feels quite classical.</small>

> **Recording down-to-earth trending tech I see every week, filtered and published here. Follow this weekly newsletter to get update notifications**

## Trending Tools

**celldock-for-mac: use cellular data, SMS and calls on your Mac**
<https://github.com/celldock/celldock-for-mac>
This one is interesting. If you have a DJI 4G module lying around, give it a try. I plan to get one to play with. You could even put in a SIM card, pair it with a module, and turn it into a portable Wi-Fi hotspot for when you are out.
<img src="https://cdn.tw93.fun/uPic/Y71cWg04.png" width="800"/>

**kooky: another macOS terminal built for AI coding**
<https://github.com/iAmCorey/kooky>
It supports workspace management in a sidebar, horizontal and vertical splits, one-click agent launch, and live agent status. You can also see Git, Node, Python and other workspace state right at the bottom of each pane.
<img src="https://cdn.tw93.fun/uPic/QqZtJW39.png" width="800"  style="margin-left:-16px"/>

**A 3D teaching exhibit of prehistoric animals**
<https://leon-made-this.work/museum/zh-CN/?animal=stegosaurus>
It looks like a parent built this prehistoric animal museum for their kid. Quite fun, worth a play.
<img src="https://cdn.tw93.fun/uPic/MZswYA27.png" width="800"/>

**JJ: Git-compatible version control that suits agents better**
<https://github.com/jj-vcs/jj>
The idea is pretty good. The underlying storage is still a Git repository, but the working copy model is simpler and fits coding agents better, so when the AI gets something wrong it is much easier to clean up.
<img src="https://cdn.tw93.fun/uPic/OAQjsa02.png" width="800" />

**How to get more out of Mole's screen and keyboard cleaning mode**
<https://mole.fit/>
A lot of people ask what to do when they also want to wipe the keyboard while cleaning the screen. It already works by default. Turn on the input protection permission for screen cleaning in Settings, then enter the cleaning mode from the logo, and it is well worth setting a shortcut while you are there. After that you can wipe your screen and keyboard comfortably with a soft cloth, and the keyboard will not register stray presses.

<table>
    <tr>
        <td width="22%">
          <img src="https://cdn.tw93.fun/uPic/zIK1HN54.png" width="220" />
        </td>
        <td width="39%">
            <img src="https://cdn.tw93.fun/uPic/NWNeHl11.png" width="390" />
        </td>
<td width="39%">
            <img src="https://cdn.tw93.fun/uPic/1vP43W17.png" width="390" />
        </td>
    </tr>
</table>

## Just Looking Around

**Baoyu's AI-native development workflow: a full retro of a real case**
<https://baoyu.io/blog/2026-08-24/ai-native-dev-workflow>
Writing code is only one step in the whole software development process, and in the AI era it is the step that changes the least and needs the least attention, because today's models are trained extremely well at coding and a simple natural language prompt is enough. The interesting changes are everywhere else: how requirements get analyzed, how solutions get designed, how prototypes get built, how tests get run. Those things "outside of writing code" are where AI-native development really changes the game.

**I have noticed my taste in food has changed a lot over the years**
It is a strange thing. All through university I thought I only ate spicy food and would never touch anything sweet, and yet after nearly ten years in Hangzhou I slowly came to like all kinds of flavors. Sweet dishes and light dishes can be really good too. Lately I have been enjoying Fujian food a lot, that fresh and tender feeling. Life works the same way: the more you go through, the more new things there are to experience, which is lovely.

<table>
    <tr>
        <td width="24%">
          <img src="https://cdn.tw93.fun/uPic/DA0Mmp59.png" width="240" />
        </td>
        <td width="38%">
            <img src="https://cdn.tw93.fun/uPic/yglVUa14.png" width="380" />
        </td>
<td width="38%">
            <img src="https://cdn.tw93.fun/uPic/hJq3Ux31.png" width="380" />
        </td>
    </tr>
</table>

**Some thoughts: keeping code maintainable in the AI era**
I want to talk from a product engineer's point of view about how to keep a product's code iterable, maintainable and free of rot in an era where all of it is written by AI.

Mole recently reached its 13th release. Looking at the project, there are roughly 110,000 lines of Swift product code, 73,000 lines of test code and 3,347 XCTest cases. That is far more testing than what I had back when I wrote business code at a company with QA to fall back on. I have always held one view: code written by AI should be tested by AI, not by people. Bringing people into that step actually slows everything down.

So I wanted to summarize, based on all that hands-on experience, the interesting parts that keep this code obedient between me and the AI.

1. Even though AI has massively sped up how fast code gets written, the product's architecture, its layering, how similar things get abstracted, and what goes where so it stays easy to extend and decouple later, all still need the engineer's own judgment. You can talk this through carefully with your best AI once the first version of the project runs, settle on the architecture, and record it in a document that can be revised and carried along as the project iterates.

2. What I rely on most is still unit tests. 1.0 had only 56 XCTest cases, and 1.13 already has 3,347, with test code at about 66% of the production Swift code. The numbers are just something I counted along the way, though. What I care about more is whether the tests cover the places where it is easy to assume things: files changing after a scan finishes, a process check failing, a command returning success while the app was never actually updated, an old task coming back late and overwriting a newer result. The normal cases are usually not hard to write. The hard part is catching the cases where the result looks fine but is already wrong.

3. When fixing bugs especially, I leave more behind. Besides fixing the problem, I add a regression test that would fail against the old code, then walk similar paths looking for the same kind of issue, and finally write down into the rules why the change was made that way. Mole now has over 1,000 commits starting with fix, and more than 900 of them landed together with tests. Many of those tests and rules came from something a user actually hit once, and perhaps that is the most valuable asset this project has right now.

4. Tests remember inputs and results, but they do not remember why an approach was abandoned. So the project also has a set of Rules that record feature boundaries, historical reasons and things not to touch: why certain files must never be deleted automatically even at the cost of missing some, why some components that look duplicated must not be merged, which system data does not belong to Mole. Too many rules eat a lot of context, so I split them by module and only load them when the related code is being changed, and turn the checks that repeat often into Skills. bugs looks for similar problems in past fixes, design-system-review checks whether the interface is drifting into a mess, and release covers signing, notarization, remote files and the update path. That way I do not have to explain everything to the AI from scratch each time.

5. Another thing that works well for me is building fewer features that serve no real purpose. Adding a setting, a compatibility branch or a background listener takes AI a few minutes now, and the state and maintenance cost left behind are the biggest source of rot. Mole now has some rules for new features: avoid adding always-on overhead, do not add new privileges or system permissions, do not keep adding settings when a sensible default exists, and do not keep expanding the scope of updates and cleanup just because a new possibility turned up. Plenty of features matter only to the developer while users do not care at all. Better to start from users and go back to users, and not multiply entities beyond necessity.

6. Make good use of GitHub Actions automation, it will be your last line of defense. Code written, running and tests green is still not the end. My project also has a set of checks for consistency across nine languages, the generated site, the Appcast, the Xcode project and public deployment files. make verify runs those together with the tests, and CI does it all again on a clean cloud machine. At release time, the local code, the Git commit, the signed installer, the files online and the update users actually receive are several different states, and I confirm them separately. I have run into cases where the source was completely correct while the old file was still being served online. Looking at one green result makes it far too easy to think you are done while something is still broken underneath.

7. If there is anything special about all of this, it is that the execution flow runs without me stepping in. Every step is executed and verified by the AI, and when something breaks the AI fixes it. What I do is put the necessary gates at the right points, make the AI verify actively and only pass on a clear result, then keep iterating on the rules, keeping them fresh, removing old logic in time, so the verification logic and the business code move up together.

Perhaps this is exactly the ability engineers most need to build in the AI era: how to make AI-written code more maintainable, clearer and easier to extend, so that half a year, a year or two years later it still has not rotted, and instead becomes more obedient and closer to what the developer has in mind, while also giving multi-agent development a stable foundation. The fun of writing code by hand is gone, but at least this makes up for some of the boredom of pure AI coding and keeps a bit of the engineer's craft alive.
