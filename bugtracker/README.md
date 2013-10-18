# Guidlines for Bug Reporting

Joinbox hosts all active projects on GitHub.com. Every sub project is hosted on a separate repository. Every repository has a Bugtracker. Bugs must be posted to that bugtracker.

## Preliminaries

- Feature Requests must not be reported to the Bugtracker. Please talk with the projects project manager about new features!
- Make sure you are using the newest version of the Software. The most recent software is on almost all our project found on the staging server.
- See in the Bugtracker if the bug already has been reported.


## Writing a clear Title

How would you describe the bug using approximately 10 words? This is the first part of your bug report a triager or developer will see. A good summary should quickly and uniquely identify a bug report. It should explain the problem, not your suggested solution.

- Good: "Cancelling a File Copy dialog crashes File Manager"
- Bad: "Software crashes"

- Good: "Down-arrow scrolling doesn't work in <textarea> styled with overflow:hidden"
- Bad: "Browser should work with my web site"


## Writing precise steps to reproduce

Steps to reproduce are the most important part of any bug report. If a developer is able to reproduce the bug, the bug is very likely to be fixed. If the steps are unclear, it might not even be possible to know whether the bug has been fixed.

Describe your method of interacting with the Software in addition to the intent of each step.

- Imprecise: "Navigate to the containing the event Darkside and press the «buy tickets button». ..."
- Precis: "Navigate to the page «http://events.ch/de/Electro-Electropop/Reitschule-Dachstock/Bern/Darkside/e-240076/», select 1 ticket and press the «buy tickets button». ..."

After your steps, precisely describe the observed result and the expected result. Clearly separate facts (observations) from speculations.

- Imprecise: "It doesn't work"
- Precise: "Instead of showing the german Checkout page, it shows the english checkout page."


## Screenshots & Stack Traces

- If you encounter a bug wich causes a part of the product not to be rendered as expected please include a screenshot of that part of the product. 
- Don't include screenshots if they don't display / help identify the problem.
- If any interactive part of the product fails / doesn work you should include all output of th developer console of your browser. See [getting Stack Traces](https://github.com/joinbox/guidelines/tree/master/bugtracker/stackTraces.md) 


## Product Version

You must always include the exact version of Browser / Operating System you were using when encountering the bug. See [Finding my Browser Version](https://github.com/joinbox/guidelines/tree/master/bugtracker/browserVersion.md) and [Finding my Operating System Version](https://github.com/joinbox/guidelines/tree/master/bugtracker/osVersion.md)

- Bad: "Firefox / Linux"
- Good: "Mozilla Firefox for Ubuntu canonical 1.0 / Ubuntu Saucy Salamander (development branch), 13.10"


## Assignment & Labels

Please don't assign the bug to anyone and don't add any labels to it. The Project Manager will triage the Bugs once a day. If it's urgent contact the Project Manager directly.