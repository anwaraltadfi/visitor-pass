# Campus Visitor Pass

A one-page classroom proof for Bay Atlantic University CMPS 570 Week 6. Visitors request visits, the assigned host approves or denies, and a guard checks approved pass codes. The proposed full system is a layered monolith. The browser proof keeps UI, application rules and repository responsibilities separate inside one HTML file.

## Links to fill after publishing

- Repository: add your actual GitHub repository URL.
- Live page: add your actual GitHub Pages URL.
- Architecture document: `docs/architecture-decision.pdf`
- Diagram: `docs/architecture.excalidraw` and `docs/architecture.png`
- Presentation: `docs/visitor-pass-presentation.pptx`

## Run

Open `index.html` directly in a modern browser. No dependencies, build, login or backend are required. Use fictional visitor names. Data lives in memory and disappears on reload. For the valid-pass demo, choose today's date.

## Security scope

The role switcher simulates identity. Application methods reject approval by Visitor, Guard, or a Host who is not assigned to the request. They check permissions regardless of which UI button calls them. This is a demonstrable rule, not real authentication: browser users can select another role or tamper with client code. The full system must supply trusted actor identity from authenticated server middleware and enforce all permissions on the server.

The proof validates names, real calendar dates and listed hosts. Codes use browser cryptographic randomness, and duplicate codes cannot be issued. A code is valid only for an approved request on its visit date. Guard responses expose only validity and date. Audit events omit visitor names and pass codes. HTTPS, database encryption, durable audit storage, throttling and retention controls belong to the proposed full system and are not implemented in this page.

## Automated service tests

Install Node.js if you do not already have it. From the project root:

```bash
node --test tests/core.test.cjs
```

The dependency-free suite contains 18 tests. It injects a fresh in-memory repository, fixed clock and actor provider. Cases cover bad input, unauthorized approval, wrong-host access, approval and denial, repeated decisions, today's and future/expired passes, restricted Guard results, audit redaction, repository mutation isolation and random-code collisions. All 18 passed during preparation. No branch-coverage percentage is claimed.

## Manual acceptance checks

1. Send a blank name, a past date and an unlisted host through the service tests. Expect rejection without stored records.
2. As Visitor, create a request for today and Admissions Office. Click **Try approval as Visitor**. Expect access denied.
3. As Host, select Student Services. The Admissions request is absent. Switch to Admissions Office, approve and copy the code.
4. As Guard, enter a wrong code, then the approved code. Expect invalid, then valid. The result must not show a visitor name.
5. Create another request, deny it, and verify no code is created.
6. Approve a future visit. The Guard must report invalid before that date.
7. Open the activity log and check that visitor names and codes are absent. Reset or reload and confirm data clears.
8. Open the published page on a phone. Check the role switcher, form, approve/deny buttons and guard result. Record the actual device/browser and result; do not claim a phone test before doing it.

## Publish with GitHub Pages

1. Create a public repository, for example `visitor-pass`, and add your teammates as collaborators.
2. Upload the **contents** of this project, so `index.html` is at the repository root. Keep `docs/` and `tests/` as folders.
3. In repository **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/(root)`, and save.
4. Wait for the published URL. Check it in a browser and on a phone.
5. Use the actual repo URL and live URL in the course form. Open the PDF through the live site, for example `https://USERNAME.github.io/visitor-pass/docs/architecture-decision.pdf`.
6. Drag `docs/architecture.excalidraw` into Excalidraw. Use **Share** to obtain your team's diagram link.
7. Upload the PPTX to PowerPoint Online or Google Slides and create a viewable presentation link. Confirm the instructor can open each link without needing your account.

## Team and contributions

Add 2–4 confirmed members and describe their real work. Each teammate should commit their own contribution with their own GitHub account.

| Member | Actual contribution |
| --- | --- |
| Muhammed Enver Altadfi | Fill after reviewing and adapting the project |
| Add teammate | Fill actual contribution |
| Optional teammate | Fill actual contribution |
| Optional teammate | Fill actual contribution |

## Presentation

Use the five slides and speaker notes for a five-minute presentation: approximately three minutes on architecture and two minutes on the live demo. `docs/presentation-script.md` contains the same timing and rehearsal instructions.

## Course source

Dr. Tsige Tessema, **Week 6 Security and Testability**, supplied lecture slides. Relevant concepts include least privilege, trust boundaries, authentication/authorization, abstract data sources, dependency injection, controllability, observability and limiting nondeterminism. The lecture cites Bass, Clements and Kazman, *Software Architecture in Practice*, fourth edition, chapters 11–12.
