# PITCH.md
Built: A disruption-care agent for Larkspur flights, with the nine given tools plus two more, next_available_day and fare_rules, served by an MCP server. We fixed the tool loop and two tool descriptions.
Does: For a customer with a disrupted flight, it looks up the booking, checks the flight status and the policy, finds rebooking options, finds the earliest day with an open seat, explains the fare rules in plain words, and hands group bookings to a human.
Number: 2,728 to 2,947 tokens in on every turn, for the whole tool list (11 tools), when we moved our two tools to the MCP server.
Safety check: It does not hold a seat, issue a voucher or book without asking the customer first. In the R8KD3F trace it offered each one and asked, and called none of them.
Next: Write our own test cases and run them after every change, and test how the agent answers an angry customer.
Still broken: On T9WN4C, a missed connection, the trace shows the agent called get_flight_status only once (LK108), and its answer never talked about the connection. In one run it also said a 150-minute delay was "over 3 hours".
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
