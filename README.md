# Pactra

**Shopping on your terms.**

Pactra is a planned browser extension that helps users move from a shopping request to a confirmed merchant order, while keeping payment bound to the exact purchase they approve.

**Status:** Registration & Idea Phase for the Airwallex Agentic Banking Hackathon. This repository documents the proposed product. The extension, payment integrations, and working demo are not yet implemented.

## The problem

A shopping request is not permission to make any purchase within a budget.

A user might approve a specific product, merchant, delivery option, and final price. During checkout, that price may change, stock may disappear, or delivery terms may differ.

An agent must recognize those changes and obtain fresh approval when they fall outside the authorized purchase. It must also distinguish a confirmed payment from an uncertain checkout attempt before retrying.

## What we will build

Pactra will provide a browser sidebar where users describe their needs, follow the agent’s progress, review purchase details, and approve or pause execution.

The agent will operate on a supported merchant website through browser automation. It will inspect products, compare options, select variants, prepare a cart, and proceed through supported checkout steps.

The first prototype will target one selected merchant. Universal compatibility across merchants is outside the initial scope.

## Planned shopping flow

1. **Describe the purchase.** The user specifies product preferences, budget, quantity, and delivery requirements.
2. **Inspect and compare.** The agent reads current product information and evaluates available options.
3. **Prepare the cart.** The agent selects variants and verifies the resulting cart.
4. **Review the purchase.** Pactra presents the merchant, products, quantities, delivery choice, final total, and intended payment method.
5. **Approve exact terms.** The user authorizes that purchase.
6. **Validate and execute.** The application checks that checkout still matches the approved scope before initiating payment.
7. **Handle changes.** The agent pauses for renewed approval if the purchase moves outside that scope.
8. **Verify the result.** Pactra checks payment and merchant order status before reporting success or considering a retry.

Merchant login and payment verification may require user intervention. The agent will pause for supported human handoffs rather than assume these steps are complete.

## Intent and approval are separate

Pactra will distinguish:

- **Purchase intent:** preferences and constraints that guide shopping.
- **Purchase policy:** rules that determine which actions are permitted.
- **Payment approval:** authorization for a specific purchase.
- **Checkout outcome:** verified evidence of payment and merchant order completion.

A general budget does not automatically authorize a changed purchase.

Where supported, we will explore virtual-card controls that enforce the chosen purchase policy. Application budget checks alone will not be presented as issuer-enforced card controls.

## Proposed architecture

| Component | Responsibility |
|---|---|
| Browser extension | Sidebar, purchase review, progress, and user controls |
| Browser executor | Read permitted page information and perform controlled shopping actions |
| Backend agent runtime | Planning, tool orchestration, execution state, and recovery |
| Policy and approval service | Validate financial constraints and bind approvals to transaction details |
| Payment adapter | Integrate supported Airwallex payment tooling |
| Transaction records | Track approved terms, attempts, payment outcomes, and order confirmation |

The browser-control approach is still under evaluation. Options include extension-native execution or a companion runtime connected to the user’s browser. Browser Use is a candidate, not a confirmed dependency.

A cloud browser will not be treated as automatically sharing the user’s local merchant session.

## Per-user boundaries

Each shopping run will belong to an authenticated application user and a specific browser session.

The backend will enforce access boundaries for shopping tasks, approvals, transaction records, and authorized payment connections. One user’s approval or payment authorization must not be reused for another user.

Merchant login, Pactra login, and payment authorization are separate. Logging in does not approve a purchase.

## Payment integration

For Approval-Bound Shopping Agent, we plan to request allowlisted Airi CLI access and follow the beta instructions supplied by Airwallex.

The proposed integration covers payment-method selection, payment mandates, transaction-specific credentials, and payment-result reporting.

The builder guide describes real merchant purchases using an Airwallex-issued card pre-funded for each team. Support for independently connected personal payment methods is not yet confirmed and is not an MVP promise.

Airwallex’s general CLI, AgentOS, and sandbox Developer MCP will not be treated as interchangeable with Airi CLI.

## Credential handling

Payment credentials will be excluded from model context, chat history, ordinary logs, and browser recordings.

A dedicated payment executor will handle credentials using the mechanism supported by the provider. Credential values will not be passed through free-form agent reasoning.

Approval validation and duplicate-payment protection will be enforced in application code. An uncertain result will trigger outcome verification or a user handoff rather than an automatic second payment.

## Hackathon alignment

**Primary starter kit:** Approval-Bound Shopping Agent.

**Complementary direction:** Intent-Bound Purchase Agent, subject to demonstrating supported card-control enforcement.

We also intend to evaluate Visa Intelligent Commerce or Trusted Agent Protocol with sponsor guidance. Access, interoperability, and Visa Award eligibility remain to be confirmed.

## MVP demonstration

The working demo should show:

- A shopping request on one supported merchant.
- Visible browser actions that prepare the cart.
- Explicit approval of final purchase terms.
- A changed-terms scenario requiring renewed approval.
- A completed purchase using authorized hackathon payment tooling.
- Verification of the payment outcome and merchant order confirmation.
- Safe handling of an uncertain checkout result without a duplicate payment.

## Dependencies to resolve

- Airi beta access, installation, and authentication.
- Supported runtime and account boundaries.
- Payment credential delivery and result-reporting requirements.
- Compatibility with the selected merchant checkout.
- Supported card-policy controls.
- Available Visa tooling and award requirements.

## Running the project

There is no runnable implementation yet. Setup instructions, configuration examples, and demo steps will be added as development progresses.

## References

- [Airwallex Agentic Banking Hackathon](https://airwallex.hackerearth.com/)
- [Airwallex Developer Lab](https://airwallexdev.com/)
- [Hackathon Builder Guide](https://airwallexdev.com/guide)
- [Airwallex API Documentation](https://www.airwallex.com/docs/api/introduction)
