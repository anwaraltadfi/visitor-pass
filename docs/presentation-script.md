# Five minute presentation

## Slide 1   0:00–0:25   Campus Visitor Pass

Our project supports a simple campus workflow. A visitor asks to visit, the assigned host approves or denies, and the guard checks the pass at the gate. We recommend a layered monolith for the full system. This page is a small proof of the workflow using simulated roles and fictional data.

## Slide 2   0:25–1:10   Architecture choice

Security and testability shaped the decision. The server provides one place to authenticate staff and validate requests. The Pass module checks permissions before changing state. Separate repository and clock interfaces make tests repeatable. Maintainability is our third quality: we can change the page or storage adapter without rewriting the approval rules. We accept that one server outage affects the whole workflow. Microservices add deployment and network complexity. Event-driven approval introduces delays and duplicate-event handling at the gate, where the decision should be immediate.

## Slide 3   1:10–2:00   Components and boundaries

The browser is untrusted. Requests cross the first trust boundary into HTTP and authentication middleware, then reach the Pass module. The module checks both the role and the assigned host before a decision. The repository interface separates the visit rules from storage. Database access crosses the second boundary into a private data tier. The modules deploy together. Guard responses contain only validity and date. The full system adds real staff authentication, HTTPS and database encryption. Our browser proof cannot provide that protection because its roles and data are controlled locally.

## Slide 4   2:00–3:00   Decisions and evidence

ADR 001 chooses a layered monolith for a small team and a short workflow. ADR 002 injects data storage and security context so tests can control them. Our security cases cover unauthorized approval and unnecessary personal-data exposure. Our testability case checks approval and expiry using a fresh fake repository and a fixed date. The supplied service suite passes 18 checks, including wrong-host access and duplicate pass codes. We do not claim that these tests prove production security. In production, authenticated middleware supplies the actor, and the server protects the same rules.

## Slide 5   3:00–5:00   Live workflow proof

Before presenting, open the page with empty data. Use a fictional name and today's local date.

1. Submit a blank name to show a clear validation error.
2. Submit **Alex Morgan**, today's date and **Admissions Office**.
3. Click **Try approval as Visitor** and show access denied.
4. Switch to **Host**. Choose **Student Services** and show no Admissions request. Switch back to **Admissions Office**, approve and copy the generated code.
5. Switch to **Guard**. Check a wrong code, then the approved code. Show invalid followed by valid, with no visitor name in the result.
6. If time permits, open the activity log and show that it contains actions without names or codes.

Close by explaining that the page demonstrates the workflow and interfaces, while the architecture document explains the server controls required for real visitors. Practice once before class and assign speaking/demo roles to actual teammates.
