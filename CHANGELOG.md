# Changelog

All notable changes to the Quotation Guidelines specification will be documented in this file.

## [1.1] - 2025-12-16

### Added
- **offer_format** field in commercial_parameters section
  - Options: itemized / package / both
  - Controls how pricing is presented in quotations
  - FIT typically uses itemized, Groups typically use package
  - Default: itemized

### Changed
- Updated version from 1.0 to 1.1
- Added changelog section to spec

### Reasoning
- During testing with real lead (Exotik Journeys FIT), identified that offer format (itemized vs package pricing) is a critical presentation preference
- Different markets and tour types have strong preferences for how pricing is displayed
- This affects quote structure and customer decision-making

---

## [1.0] - 2025-12-02

### Initial Release

#### Core Sections
- Basic Information (market, agent, tour type, language)
- Accommodation (star rating, location, brand strategy)
- Meals & Restaurants (conditional - groups only)
- Transport & Transfers (FIT vs Group specific fields)
- Activities & Tours (FIT vs Group specific fields)
- Commercial Parameters (margin, currency, FOC brackets)

#### Key Features
- Conditional display based on tour type
- Integration with agent settings (preferred brands, blacklists, margins)
- Priority hierarchy (lead data > guidelines > agent settings > defaults)
- Comprehensive validation rules
- Business logic documentation

#### Design Decisions Made
- **Removed sales staff field** - roster changes make this unsustainable
- **Removed group size range** - handled by lead parsing, not a preference
- **Removed PPPN budget** - too variable by city (Paris vs Budapest)
- **Simplified location priority** - binary yes/no instead of multiple options
- **Removed room config preference** - handled by lead parsing
- **Hotel brands reference agent settings** - no data duplication
- **Removed free text fields** - structured inputs only to prevent AI hallucination
- **Margin % inherits from agent settings** - optional override in guidelines
- **Activity intensity sets constraint** - packed=4, moderate=3, relaxed=2 activities/day

#### Files Included
- quotation_guidelines_spec.json - technical specification
- quotation_guidelines_mockup.html - interactive UI mockup

---

## Future Considerations

### Potential Additions (Not Yet Scheduled)
- **Seasonal variations** - different guidelines for peak vs off-peak seasons
- **Budget bands** - different guidelines for luxury vs budget clients within same market
- **Destination-specific overrides** - special rules for specific cities or regions
- **Multi-currency display** - show pricing in multiple currencies simultaneously
- **Template selection** - different quote templates/styles per market
- **Language-specific formatting** - date formats, number formats per language
- **Activity category preferences** - cultural vs adventure vs food experiences ratio
- **Sustainability preferences** - eco-friendly hotels, carbon offset options

### Known Limitations
- Guidelines don't include actual pricing (handled by rate sheets module)
- Rule logic layer not yet implemented (fallback hierarchies, conflict resolution)
- No version control within guidelines (can't track guideline changes over time)
- Single active guideline per market/agent/tour-type combo (no A/B testing capability)

---

## Version History Summary

| Version | Date | Key Changes |
|---------|------|-------------|
| 1.1 | 2025-12-16 | Added offer_format field |
| 1.0 | 2025-12-02 | Initial release |
