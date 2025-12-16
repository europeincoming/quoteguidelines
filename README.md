# Quotation Guidelines - Nova ERP

## Overview

Quotation Guidelines are market/agent/tour-type specific parameters that drive AI-powered quote generation in Nova ERP. They tell the AI how to build itineraries and price tours based on business rules and customer preferences.

**Purpose:** Enable automated quotation generation by capturing the decision-making logic that sales staff use when manually creating quotes.

**Current Version:** 1.1

---

## Repository Contents

- **quotation_guidelines_spec.json** - Complete technical specification for development team
- **quotation_guidelines_mockup.html** - Interactive UI mockup showing conditional display and user flow
- **examples/** - Sample guidelines and test scenarios
- **CHANGELOG.md** - Version history and updates

---

## How It Works

### Hierarchy

When AI processes a lead, it selects guidelines in this priority order:

1. **Lead Parsing Data** (highest priority) - explicit customer requirements
2. **Agent-Specific Guidelines** - if agent has custom guidelines
3. **Market-Specific Guidelines** - fallback to market defaults
4. **System Defaults** - last resort fallback

**Critical Rule:** Customer requirements from lead parsing ALWAYS override guideline defaults.

### Example Flow

```
Lead arrives → Parse email → Identify market/agent/tour type 
→ Query guidelines → Load parameters → Build itinerary → Price it → Generate quote
```

---

## Key Features

### Conditional Display by Tour Type

The form adapts based on tour type selection:

**FIT (Free Independent Traveler):**
- Shows: Train preferences, airport transfers, SIC tour options, activity mix
- Hides: Meal planning, coach logistics, FOC brackets

**Group Tours:**
- Shows: Meal planning, coach logistics, FOC brackets, guide requirements
- Hides: FIT-specific transport options

### Integration Points

**Agent Settings Integration:**
- Preferred hotel brands (reference, no duplication)
- Blacklisted hotels (reference, no duplication)
- Margin percentage (inherit unless overridden)

**Lead Parsing Integration:**
- Customer-specified hotels override guideline defaults
- Budget constraints override star rating preferences
- Group size triggers appropriate FOC brackets

---

## Field Categories

### Basic Information
- Guideline Title
- Status (Active/Inactive)
- Market (multi-select)
- Agent (multi-select, optional)
- Tour Type (FIT / Group / School / Corporate / Event)
- Language

### Accommodation
- Star Rating (3/4/5-star)
- City Center Priority (Yes/No)
- Hotel Brand Strategy (chain-only / chain-preferred / mix / local-preferred / budget-optimized)
- Breakfast Included (Yes/No)
- References to agent's preferred brands and blacklists

### Meals & Restaurants (Group Tours Only)
- Meal Plan (breakfast-only / half-board / full-board)
- Cuisine Type (indian-only / international / local / mix)
- Indian Meal Template (for Indian market groups)
- Dietary Requirements (halal, vegetarian, jain, etc.)
- Meal Style (buffet / set menu)
- Restaurant Type Preference
- Packed Meal Rules

### Transport & Transfers
**FIT-specific:**
- Train Priority (preferred / flexible / avoid)
- Train Class (1st / 2nd / flexible)
- Airport Transfers (private / shared / public)
- SIC Tours Acceptable (yes/no)

**Group-specific:**
- Coach Category (premium / standard / basic / budget)
- Max Driving Hours per Day
- Driving Time Window
- Driver Tips (EUR/GBP per person per day)

### Activities & Tours
**FIT-specific:**
- Activity Mix (SIC-heavy / balanced / self-guided / private)
- Hop-On-Hop-Off Acceptable (yes/no)
- Optional Activities Strategy (suggest / include / exclude)

**Group-specific:**
- Activity Type (guided-only / mix / cultural-heavy / adventure / educational)
- Guide Requirements (half-day / full-day / specific attractions / none)

**Common:**
- Activity Intensity (packed / moderate / relaxed)
- Free Time Allocation (minimal / moderate / generous)

### Commercial Parameters
- Margin Percentage (inherit from agent settings unless overridden)
- Pricing Currency (EUR / GBP / USD)
- Offer Format (itemized / package / both) **[NEW in v1.1]**
- FIT: Non-Refundable Pricing (yes / no / flexible)
- Group: FOC Brackets (table with pax ranges and FOC allocation)

---

## Business Logic Examples

### FIT Example: Canada Market

**Guideline Parameters:**
- Market: Canada
- Tour Type: FIT
- Star Rating: 4-star minimum
- City Center: Priority (strict)
- Hotel Strategy: Chain preferred
- Train Class: 1st class
- Airport Transfers: Private
- Offer Format: Itemized

**Lead Override:**
Customer requests: "Mandarin Oriental in Lucerne, Intercontinental in Amsterdam"

**AI Decision:** Override star rating to 5-star. Quote specific luxury hotels as requested. Guideline provides fallback strategy but lead requirement wins.

---

### Group Example: Indian Market

**Guideline Parameters:**
- Market: India
- Tour Type: Leisure Group
- Star Rating: 3-4 star mix
- Meal Plan: Half board (all dinners)
- Cuisine: Indian-only
- Indian Menu: 1 non-veg curry, 2 veg curries, rice, dal, naan, etc.
- Coach: Standard category
- Guide: Half-day in major cities
- FOC Brackets: 1 FOC single from 15 pax
- Offer Format: Package

**Lead:** 25 pax group, 7N/8D Italy tour

**AI Decision:** Apply guideline parameters. Calculate pricing with 1 FOC (25 pax bracket). All dinners at Indian restaurants with standard menu. Package pricing per person.

---

## What's NOT in Guidelines

These are handled elsewhere in the system:

- **Rate Sheets** - actual pricing for hotels, activities, transport (separate module)
- **Supplier Contracts** - FOC brackets from suppliers, contracted rates
- **Rule Logic Layer** - underlying decision trees (e.g., "if hotel unavailable, downgrade budget 10% before changing location")
- **Lead Parsing** - extracting data from email inquiries
- **Completeness Assessment** - checking if lead has enough info to quote

---

## Validation Rules

### Required Fields (All Guidelines)
- Guideline Title (unique)
- Status
- Market
- Tour Type
- Language
- Star Rating
- City Center Priority
- Hotel Brand Strategy
- Breakfast Included
- Activity Intensity
- Free Time Allocation
- Pricing Currency
- Offer Format

### Conditional Required (Groups Only)
- Meal Plan
- Cuisine Type
- Meal Style
- Restaurant Type Preference
- Coach Category
- Max Driving Hours
- Driving Time Window
- Driver Tips
- Activity Type
- Guide Requirements
- FOC Brackets

### Conditional Required (FIT Only)
- Train Priority
- Train Class
- Airport Transfers
- SIC Tours Acceptable
- Activity Mix
- HOHO Acceptable
- Optional Activities Strategy
- Non-Refundable Pricing

### Business Rules
- FOC brackets must not overlap
- Guideline title must be unique
- Market + Agent + Tour Type combination must be unique
- If cuisine type = indian-only or mix, indian_meal_template is required

---

## Development Status

### Completed ✅
- Field definitions and validation rules
- Conditional display logic (FIT vs Group)
- Integration requirements (agent settings, lead parsing)
- Data structure and JSON schema
- UI mockup with interactive conditional display
- Offer format field (itemized/package/both)

### In Progress ⏳
- Backend API development
- Database schema implementation
- Agent settings integration
- Lead parsing integration

### Not Started ❌
- Rule logic layer (underlying decision trees)
- AI workflow nodes (itinerary building, pricing calculation)
- Rate sheets integration
- Quote text generation

---

## Testing Approach

1. Create guideline for specific market/agent/tour type
2. Feed parsed lead data to AI with guideline parameters
3. Generate quote
4. Compare AI output to human-generated quotes
5. Identify gaps and hallucinations
6. Refine guidelines and rule logic
7. Iterate until AI quotes match human quality

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for detailed version history.

---

## Questions or Issues?

Contact the Nova development team or open an issue in this repository.

---

**Last Updated:** December 2025  
**Maintained by:** Europe Incoming Development Team
