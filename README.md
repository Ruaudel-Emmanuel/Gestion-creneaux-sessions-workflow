Session Slot and Registration Management Workflow

This n8n workflow automates the handling of session registrations, slot availability checks, booking confirmation, reminder sending, and fallback to a waiting list when no seats remain.[file:1]
Overview
The workflow is named Brique 16 - Gestion de créneaux et de sessions and is described in the metadata as a demonstration workflow for managing session slots and sessions.[file:1]
It starts from a manual trigger, creates a sample registration request for a participant, checks whether seats are still available for a predefined session, and then follows one of two paths: confirm the booking or propose another time slot.[file:1]
What the workflow does
Receives a registration request containing a participant name, email, phone number, request type, and status.[file:1]
Looks up a sample available session with a session ID, date/time slot, maximum seats, and remaining seats.[file:1]
Verifies whether at least one seat is still available using an `IF` condition on `places_restantes`.[file:1]
If a seat is available, decreases the remaining seat count, sets the status to confirmed, and generates a confirmation message.[file:1]
Adds a confirmation flag and a video meeting link.[file:1]
Waits one hour before sending a reminder message to the participant.[file:1]
Produces a final success status indicating that the participant is confirmed and that the session was organized automatically, with an estimated time saving of 30 to 60 minutes per organized session.[file:1]
If no seat is available, switches the registration to a waiting-list status and prepares a message proposing another slot.[file:1]
Ends with a summary output node for both possible branches.[file:1]
Workflow structure
Step	Node name	Purpose
1	Déclencheur manuel	Starts the workflow manually for testing or demonstration purposes.[file:1]
2	Demande d'inscription	Builds a sample participant registration payload.[file:1]
3	Chercher créneau disponible	Sets the target session data and remaining seats.[file:1]
4	Place disponible ?	Checks whether `places_restantes` is greater than 0.[file:1]
5A	Réserver la place	Confirms the booking and reduces remaining capacity by 1 through JavaScript code.[file:1]
6A	Envoyer confirmation	Adds a confirmation status and video call link.[file:1]
7A	Attendre avant rappel	Waits 1 hour before the reminder step.[file:1]
8A	Envoyer rappel	Prepares the reminder message.[file:1]
9A	Session organisée	Outputs the business result and estimated time saved.[file:1]
5B	Proposer autre créneau	Moves the participant to the waiting list and prepares an alternative-slot message.[file:1]
10	Synthèse créneaux	Produces the final workflow output for either branch.[file:1]
Sample input data
The registration example includes the participant Claire Martin, the email `claire.martin@example.com`, the phone number `0677889900`, and a request labeled `Inscription atelier collectif` with status `reçu`.[file:1]
The session example uses `SESSION-28-05-A`, the slot `28/05/2026 - 14:00`, a maximum capacity of 8 seats, and 3 remaining seats before booking.[file:1]
Booking logic
The decision point checks whether the numeric value of `places_restantes` is greater than 0.[file:1]
When the condition is true, the Code node subtracts one seat from the remaining capacity, changes the status to `confirmé`, and builds the message `Votre place est réservée pour la session ...`.[file:1]
When the condition is false, the workflow sets the status to `liste d'attente` and returns the message `Aucune place disponible sur ce créneau, proposition d'un autre créneau.`.[file:1]
Business value
This workflow is useful for training sessions, workshops, onboarding calls, or any group event where seat availability must be managed automatically.[file:1]
It helps reduce manual follow-up, standardize participant communication, and save an estimated 30 to 60 minutes per organized session according to the workflow output data.[file:1]
Notes
This is a demonstration workflow built with `Set`, `If`, `Code`, and `Wait` nodes, so the current version uses hardcoded sample data rather than a live source such as Airtable, Google Sheets, Calendly, or a CRM.[file:1]
In a production version, the manual trigger and static values could be replaced with a form submission, webhook, database query, or scheduling tool integration.[file:1]
How to use
Import the JSON file into n8n.[file:1]
Open the workflow and review the sample values used in the registration and session nodes.[file:1]
Run the workflow manually to test both outcomes by changing the `places_restantes` value.[file:1]
Replace the demo data with real data sources and notification channels for production use.[file:1]
