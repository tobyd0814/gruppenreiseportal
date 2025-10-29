# Gruppenreiseportal - Claude Instructions

## Project Overview

This is a specialized platform for organizing and booking travel for large groups (e.g., school classes, choirs, orchestras, sports teams). The system digitalizes and centralizes the complex logistical, financial, and safety-related tasks typically handled manually by group organizers.

**Core Purpose:** Enable group organizers (e.g., teachers) to plan trips, receive proposals from accommodation providers, manage automated split payments among group members, coordinate logistics, and ensure safety during travel - all through a centralized platform.

**Key Features:**
- **Request for Proposal System:** Organizers specify trip requirements (travelers, room configurations, special needs) and receive tailored offers from property managers
- **Automated Split Payments:** Individual payment links for each group member with installment options and automated tracking/reminders
- **Collaborative Trip Planning:** Live-updated itineraries, booking of additional services (buses, catering, activities)
- **Digital Document Management:** Electronic permission forms with legally-binding digital signatures
- **Safety & Communication:** Real-time group map, geofencing alerts, digital headcounts, SOS button, role-based communication channels

**User Roles:**
1. **Group Organizer:** Plans trips, manages requests for proposals, coordinates payments and logistics
2. **Large-Format Property Manager:** Reviews requests for proposals, submits offers, manages bookings
3. **Group Members:** Make payments, access trip information, use safety features
4. **Chaperones:** Monitor group during travel, use safety and communication tools

## Engineering Standards

**IMPORTANT:** This project follows strict engineering standards and protocols. All development must adhere to the guidelines in [ENGINEERING.md](ENGINEERING.md), which covers:

- Core principles (Enterprise-grade mandate, Security, Immutability)
- Test-Driven Development (TDD) methodology
- Version control and Git workflow (Conventional Commits)
- CI/CD pipeline and quality gates
- Code style conventions
- API standards
- Database conventions
- Logging and error handling

**Key Requirements:**
- **TDD is mandatory** - Write failing tests first, then implement
- **Conventional Commits required** - Format: `<type>(<scope>): <subject>`
- **85% minimum code coverage** - All services must maintain high test coverage
- **Security first** - OWASP Top 10 compliance, no secrets in code
- **Immutable infrastructure** - No manual changes to environments

## Tech Stack

*(To be defined based on project requirements)*

## Project Structure

*(To be defined)*

## Documentation Standards

### Keep Updated
- **README.md:** User-facing documentation, getting started guide
- **AGENTS.md (this file):** AI-specific instructions, conventions

### When to Update AGENTS.md
**IMPORTANT:** When errors occur during implementation, suggest changes to AGENTS.md to prevent future occurrences.

Examples:
- Common mistakes → Add to conventions
- Missing context → Add to project structure
- Unclear requirements → Clarify in relevant section

---

**Last Updated:** 2025-10-30
**Version:** 0.1.0
