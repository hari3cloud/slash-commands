> **DEPRECATED (2026-06-04).** `/fleet-orchestrate` is retired. Its value — a waved build with a
> MANDATORY adversarial verification pass — is now inside `/poc-builder`: WF-2 (`poc-build`) runs the
> waved build with the API-contract hard gate, and WF-3 (`poc-harden-harvest`) runs the mandatory
> adversarial-verify pass (multiple skeptics per finding), self-heal, and visual regression.
>
> **Use `/poc-builder {ClientName}` instead.** See `_System/skills/poc-builder.md` and
> `Knowledge/Practice/AI-Engineering/docs/PoC-Builder-Guide.md`.
