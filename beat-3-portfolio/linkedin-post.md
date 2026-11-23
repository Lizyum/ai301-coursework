I just proved, instead of assumed, a real accessibility bug in a 20-year-old open source chemistry app, and built the checks that kept my own AI-assisted work honest along the way.

Avogadro is a desktop tool chemists use to build and visualize molecules. One of its dialogs colors text with fixed colors instead of adapting to light or dark mode, so some text barely passes a contrast test and one error message relies on color alone to get your attention. I claimed a piece of that open issue, reproduced the bug myself in a headless Linux build, and measured the actual contrast ratio of the broken text — it matched what the maintainers had reported almost exactly.

Along the way I built small tools to grade candidate issues, check my plan before writing any code, and double-check my own notes rather than trust them. One of those checks caught a note where I had the fix's location wrong, before it could steer me the wrong way.

The fix itself is still ahead of me: the bug is confirmed, the plan is reviewed, and I'm about to open my first pull request to an open source project. It's taught me to verify before I claim something is true, especially when AI tools are doing part of the work.

https://github.com/OpenChemistry/avogadrolibs/issues/3187#issuecomment-6785796862

#OpenSource #SoftwareEngineering #AI
