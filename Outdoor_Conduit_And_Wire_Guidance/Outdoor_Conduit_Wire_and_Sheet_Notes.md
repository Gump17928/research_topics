# Outdoor Structure Power: Conduit, Wire Size, and Sheet Notes

> **Starting-point guide, not a design approval.** Numbers are from memory of NEC (NFPA 70) tables and typical Navy/NAVFAC practice. Verify each item against the **NEC edition adopted by the contract**, the **UFC/NAVFAC criteria named in the contract**, and the owner/AHJ. Items marked **[VERIFY]** are the ones most likely to differ by edition or installation. Related: [05_Outdoor_Electrical_Design_Considerations.md](../Revit_CUI_Electrical_Sheets_Workflow/05_Outdoor_Electrical_Design_Considerations.md).

## 0. Assumptions (edit these as they get confirmed)

| # | Assumption | Confidence | Why it matters |
|---|---|---|---|
| A1 | Outdoor, exposed to weather, possibly salt/marine air | Fairly sure | Drives conduit material and enclosure rating |
| A2 | Hard to access, so you want low maintenance and no re-pulls | Sure | Drives upsizing, spares, pull string |
| A3 | 2 loads at 115/120 VAC, plus 1 **azimuth drive motor run by a motor controller unit (MCU)** | **Known (rev 2)** | The feeder serves the **MCU input**, not the motor directly. See Section 1A |
| A3a | The MCU nameplate/datasheet sets the supply voltage, phase, input current, and max OCPD. The motor's own phase/voltage now only matters on the MCU-to-motor leg | Sure | Replaces the old "motor might be single or three phase" unknown |
| A3b | MCU-to-motor leg is a **separate circuit** (drive output cable) and the drive also has **control/feedback wiring** (encoder/resolver, limit switches, brake, thermal, comms, E-stop) | Fairly sure | Needs its own raceway and cable type, not just "3 more wires" |
| A3c | MCU location (at the structure near the drive, or remote/indoors) is **not yet known** | Unknown | Sets the motor-cable length, which is limited by the drive |
| A4 | Source is likely 208Y/120 V 3-phase 4-wire (or 480Y/277 V with a transformer) **[VERIFY]**. Now confirm against the MCU input rating | Unsure | Decides if one feeder can serve everything |
| A5 | Run length unknown. Now two lengths matter: source to panel/MCU, and MCU to motor | Unknown | Voltage drop can control wire size; motor-cable length can be limited by the drive |

## 1. Recommended approach: one feeder + a local outdoor panel

> **Rev 2 change:** the motor is an azimuth drive fed by an MCU. Power goes **source -> panel -> MCU -> motor**. Size and protect the supply from the **MCU's input ratings**, and treat the MCU-to-motor cable and the control/feedback wiring as separate circuits (Section 1A). If the MCU data sheet is not in hand yet, the original "works for any voltage/phase" approach below still applies as a fallback.

1. Run **one feeder** from the source to a **small outdoor panelboard or disconnect enclosure at the structure** (NEMA 4X nonmetallic or stainless in marine air; NEMA 3R only for mild inland sites **[VERIFY]**).
2. Make the feeder **3-phase, 4-wire + separate equipment grounding conductor (EGC)**, usually 208Y/120 V, **unless the MCU input is single-phase** (many small MCUs take 115/230 V single-phase; confirm on the data sheet).
   - The 115 VAC loads come off phase-to-neutral (put them on different phases).
   - The **MCU input** connects to the poles the data sheet requires (2 or 3 poles, and the neutral only if the MCU needs it).
3. Short branch runs from the panel to each piece of equipment. Provide a **lockable disconnect in sight of the MCU** (and of the motor if the MCU is remote or the motor can be energized independently; NEC 430.102, 430.109 **[VERIFY]**). Use the MCU manufacturer's required disconnect/branch protection (some require specific fuse classes).
4. If the MCU requires **480 V** (or another voltage not at the source), a separate feeder or a dry-type transformer is needed. Flag this as an open item rather than assuming it away.

Why this helps when access is hard: you only pull one feeder up the structure, and the MCU's final input details can be settled with a branch change at the panel.

## 1A. What the MCU changes

| Topic | Before (bare motor) | Now (MCU-driven azimuth drive) |
|---|---|---|
| Circuit sized from | Motor FLA in NEC 430.248/430.250 | **MCU rated input current** (nameplate/data sheet). NEC 430.122: supply conductors to a power conversion system/drive at least **125% of its rated input current** **[VERIFY]** |
| Overcurrent protection | 430.52 motor branch rules | **MCU manufacturer's max fuse/breaker** (and type) on the nameplate, plus 430.130/430.131 as applicable **[VERIFY]**. The drive provides motor overload protection (430.126), so no separate overload device is usually needed if the drive is listed for it |
| Motor wiring | Conductors from the panel | **MCU output cable** to the motor: sized at **125% of motor FLA** (430.22) from the motor nameplate; always **3 conductors + ground** for a 3-phase motor, even if the MCU input is single-phase |
| Conductor type on drive output | THWN-2 | Prefer **VFD-rated shielded cable** (e.g., symmetrical 3 power + 3 ground, XLPE) when the manufacturer calls for it. Check **max cable length** and the need for **output filters (dV/dt, reflected-wave)** |
| Raceway | One conduit for power | **Drive output in its own conduit.** **Control/signal/feedback (encoder, resolver, limit, brake, thermal, comms)** in a **separate raceway** from the power conductors, or shielded cable per the manufacturer (NEC 300.3(C), 725.136) **[VERIFY]** |
| Grounding / EMI | Standard bonding | Provide the **EGC with each circuit**, **360-degree shield terminations**, and bond the MCU, motor frame, and structure per the manufacturer. Drive noise can disturb encoder and comms signals |
| Neutral | Sized as the phase conductors | Do **not** connect the neutral to a 3-phase MCU unless its data sheet requires it; keep the neutral for the 115 V loads. Drives are nonlinear, so keep the feeder neutral full size |
| Surge / lightning | Optional | Add an **SPD at the MCU input** if the structure is tall or isolated; drives and encoders are surge-sensitive **[VERIFY]** |
| Moving structure | Fixed run | **Azimuth rotation:** slip ring or cable wrap/loop at the rotating joint; use **flexible, UV-/oil-/flex-rated cable** (or LFMC with listed fittings) across the rotating part, and set the cable loop for the full rotation range and travel stops |
| Arc flash / labeling | Motor label | Label the **MCU** with its ratings; consider arc-flash and short-circuit rating (SCCR) of the MCU on the sheet |

Layout decision (A3c):
- **MCU at the structure beside the drive:** short motor cable (usually no filter needed), so most of the run length is the feeder.
- **MCU remote/indoors:** the **motor cable could be long**. Confirm the manufacturer's max motor-lead length (long leads can need output reactors/filters, larger conductors for voltage drop, and a bigger conduit), and the encoder cable length limit.

## 2. Question 1a: What conduit?

| Segment | Suggested | Notes |
|---|---|---|
| Exposed on structure, wet and corrosive | **Rigid galvanized steel (RMC)**. **PVC-coated RMC** or **stainless** if marine/chemical **[VERIFY]** | Navy criteria generally favor RMC outdoors and restrict EMT to dry indoor areas **[VERIFY UFC 3-501-01 and the contract]**. Use threaded, listed, wet-location fittings. |
| Final connection to the motor/equipment (vibration, alignment) | **Liquidtight flexible metal conduit (LFMC)**, short length, listed fittings | Use the corrosion-resistant jacket type. Stay within the length the NEC allows. **Across the rotating azimuth joint**, use flexible cable/slip ring per the drive vendor, not rigid conduit. |
| MCU-to-motor (drive output) | Own conduit (RMC, or LFMC at the end), **separate from the feeder and from signal wiring** | Keep length within the manufacturer's limit. Shielded cable shield and ground continuity must be kept through boxes and fittings. |
| Control/feedback (encoder, resolver, limits, brake, thermal, comms) | **Separate conduit** (small, e.g., 3/4 in RMC) or shielded cable run apart from power | Don't share a raceway with drive output or feeder conductors unless the manufacturer allows it. Add one **spare conduit/pair count** since access is hard. |
| Underground portion (if any) | **Concrete-encased PVC Sch 40/80**, steel elbows and risers | Burial depth per NEC Table 300.5 and the Navy standard (often deeper) **[VERIFY]** |
| Minimum trade size | **3/4 in** (typical Navy minimum) **[VERIFY]** | |

Also include:
- **No dissimilar-metal contact** (aluminum conduit in concrete or marine air is often prohibited **[VERIFY]**).
- **Expansion/deflection fittings** where the run crosses structure joints, and on long runs (PVC especially).
- **Drain / breather fittings** at low points; avoid water traps.
- **Support spacing** per NEC 344 (RMC); vertical conductor supports per NEC 300.19 for tall rises.
- **Pull/junction boxes** for long runs or more than 360 degrees of bends between pull points.
- **Spare capacity:** go one trade size up, or add a **spare capped conduit with pull string**. Cheap now, expensive later when access is difficult.

## 3. Question 1b: What wire size?

### 3.1 Type
- **Copper, THWN-2** (90 °C, wet-location rated) in conduit outdoors. Aluminum is often restricted by Navy criteria **[VERIFY]**.
- **Stranded**, **minimum #12 AWG** for power (typical Navy minimum **[VERIFY]**).
- **Green insulated EGC** pulled in the conduit, even though RMC is a permitted ground path (Navy/industrial practice typically requires it **[VERIFY]**). Size per NEC Table 250.122 (increase proportionally if the phase conductors are upsized for voltage drop, 250.122(B)).
- Neutral: same size as the phase conductors (mixed loads, possibly non-linear).

### 3.2 Sizing steps
1. **115 VAC branch circuits:** 20 A breaker with **#12 Cu** is the usual minimum. Increase for voltage drop on long runs.
2. **MCU (azimuth drive) supply:** size the conductors at **125% of the MCU rated input current** (NEC 430.122 **[VERIFY]**) and the breaker/fuse per the **MCU nameplate maximum OCPD**. Do **not** use the 430.250 motor table for the supply side.
2a. **MCU-to-motor leg:** size at **125% of the motor nameplate/NEC-table FLA** (NEC 430.22) and check **voltage drop and the manufacturer's maximum cable length**. Use **VFD-rated cable** if the manufacturer calls for it. If the MCU is **not** in the design yet, use Section 3.3 (motor tables) as the fallback.
3. **Feeder:** 125% of the largest load (the MCU input current, in place of the motor FLA, per 430.24/430.122) + 100% of the other loads, with 125% on any continuous non-motor loads **[VERIFY]**.
4. **Corrections:** ambient temperature (NEC 310.15(B)) and more than 3 current-carrying conductors (310.15(C)(1)). Derate from the **90 °C** ampacity, but the result must not exceed the **75 °C** (or equipment-terminal) ampacity (NEC 110.14(C)). A sun-heated raceway adder applies only to raceways mounted close above a roof **[VERIFY edition]**.
5. **Voltage drop:** target 3% branch, 5% total (NEC informational notes; Navy may be stricter **[VERIFY]**). On a long outdoor run this often forces a larger size than ampacity does.
   - 3-phase: `VD = 1.732 x I x R x L / 1000`; single-phase: `VD = 2 x I x R x L / 1000` (R in ohm/1000 ft, L one-way in ft).
   - NEC Ch. 9 Table 9, Cu in steel conduit, ohm/1000 ft: #12 = 2.0, #10 = 1.2, #8 = 0.78, #6 = 0.49, #4 = 0.31, #3 = 0.25, #2 = 0.19, #1 = 0.15, 1/0 = 0.12.
   - The calculator in [Wire_Size_Calcs](../../Wire_Size_Calcs/README.md) can run this for you.

### 3.3 Motor FLA lookup (fallback, and for the MCU-to-motor leg; NEC 430.248 / 430.250, typical values; confirm in your edition)

| HP | 1-ph 115 V | 1-ph 230 V | 3-ph 208 V | 3-ph 230 V | 3-ph 460 V |
|---|---|---|---|---|---|
| 1 | 16 | 8.0 | 4.6 | 4.2 | 2.1 |
| 3 | 34 | 17 | 10.6 | 9.6 | 4.8 |
| 5 | 56 | 28 | 16.7 | 15.2 | 7.6 |
| 10 | n/a | 50 | 30.8 | 28 | 14 |

### 3.4 Worked example (placeholder numbers; replace with real ones)
- Two 115 V loads at **15 A** each, one MCU with a **208 V, 3-ph, 20 A rated input** (placeholder) driving an azimuth motor with **10 A FLA** (placeholder).
- MCU: 20 x 1.25 = **25 A**. Others: 15 + 15 = 30 A. Conservative feeder total: **~55 A** (a phase-balanced calculation can be lower).
- Starting point: **#6 Cu THWN-2** (75 °C ampacity 65 A), 60 A breaker (must not exceed the MCU's max OCPD), **#10 Cu EGC** (Table 250.122, 60 A).
- Conduit fill: 4 x #6 THWN-2 (0.0507 in2 each) + 1 x #10 (0.0211 in2) = 0.224 in2. 3/4 in RMC at 40% fill = 0.213 in2 (too tight). Use **1 in RMC** (0.346 in2 at 40%), or **1-1/4 in** for pulling room and spare capacity. Check areas in NEC Ch. 9 Tables 4 and 5.
- Voltage drop at 150 ft one-way, 55 A, #6: 1.732 x 55 x 0.49 x 150 / 1000 = **7.0 V = 3.4% of 208 V**. Marginal, so **#4** (about 2.1%) is the safer choice.
- MCU-to-motor (separate): 10 x 1.25 = 12.5 A, so **#12 Cu** minimum (20 A), 3 conductors + ground, in its own conduit. Upsize if the motor cable is long, per the manufacturer's length and drop limits.

## 4. Simple CYA general notes for the Revit sheet

Adjust the wording to match the project's note style. Put them in a **General Notes** block, and use numbered **keynotes** on the plan view for items specific to the outdoor equipment.

### 4.1 General notes
1. All work shall comply with the **current edition of NFPA 70 (NEC)**, the applicable **UFC / NAVFAC criteria**, and the contract specifications. Where they conflict, the more stringent requirement governs.
2. Drawings are **diagrammatic**. Conduit routing, pull/junction box locations, and elevations are approximate. **Field-verify all dimensions, routing, and existing conditions before installation.**
3. Contractor shall **coordinate routing and attachment** with structural, mechanical, and other trades. **No drilling, welding, or cutting of structural members** without written approval from the responsible engineer.
4. **Conduit and wire sizes shown are based on assumed loads** and shall be verified against **actual equipment nameplate data** (voltage, phase, FLA/MCA/MOCP, SCCR) before ordering. Notify the engineer of any discrepancy.
5. **Azimuth drive and motor controller unit (MCU):** supply conductors, overcurrent protection, and disconnect are sized from the **MCU nameplate/manufacturer data**. MCU-to-motor conductors are sized from the **motor nameplate**. Contractor shall submit the MCU and motor data sheets for verification **before ordering conduit and wire**.
5a. **Drive output and signal wiring:** MCU-to-motor conductors, and encoder/limit/brake/thermal/communication wiring, shall be installed in **separate raceways** from each other and from power feeders, using the **cable type, shield termination, grounding, and maximum length** required by the manufacturer. Cross power and signal wiring at right angles.
5b. **Rotating joint:** wiring across the azimuth axis shall use the slip ring or flexible cable specified by the drive manufacturer, with strain relief and a cable loop sized for the **full travel range**. Verify at both travel stops before energizing.
6. **Voltage drop** is based on the lengths shown. If the field-verified length exceeds that shown, notify the engineer before installing conductors.
7. All exterior raceways, fittings, boxes, supports, and hardware shall be **listed for wet / corrosive locations** and **compatible with each other** (no dissimilar-metal contact). Enclosures shall be **NEMA 4X** unless noted.
8. Provide **lockable disconnects within sight** of motor/equipment where required by the NEC. Provide an **equipment grounding conductor** in all raceways, **bond all metallic raceways and boxes**, and bond to the structure grounding system as required.
9. Provide **expansion/deflection fittings** at structural expansion joints and **drain fittings** at low points. Seal **all penetrations** to maintain weather and fire ratings.
10. Provide a **pull string** in all spare conduits. Cap and label spares.
11. **Label** all equipment, panels, disconnects, and junction boxes with the equipment IDs shown on the drawings (arc-flash and warning labels as required).
12. Contractor shall **verify available fault current** and provide equipment with adequate **AIC / SCCR** ratings.
13. **Outages / LOTO:** Coordinate all outages and lockout/tagout with the station before work. Work at height requires fall protection and the hazard analysis required by the contract.
14. **Existing conditions** are from record drawings and may not be exact. Notify the engineer if conditions differ.

### 4.2 Assumption note (very useful CYA)
> **ASSUMPTIONS:** (1) Source is 208Y/120 V, 3-phase, 4-wire. (2) MCU is [__] (model), [__] V, [__]-phase, [__] A rated input, [__] A max OCPD. (3) Azimuth motor is [__] HP, [__] V, [__] A FLA, located [__] ft from the MCU. (4) Two 115 VAC loads at [__] A each. (5) One-way feeder length is [__] ft. (6) Ambient temperature is [__] deg C. **If any of these change, conductor, conduit, and protective device sizes shall be re-verified.**

### 4.3 Suggested Revit schedule / detail table (placeholders)
| Tag | From | To | Conduit | Conductors | EGC | Notes |
|---|---|---|---|---|---|---|
| F-1 | Source | Panel at structure | 1-1/4" RMC | 4 #4 THWN-2 | 1 #8 | Feeder (verify) |
| B-1 | Panel | 115 V load 1 | 3/4" LFMC final | 2 #12 | 1 #12 | 20 A |
| B-2 | Panel | 115 V load 2 | 3/4" LFMC final | 2 #12 | 1 #12 | 20 A |
| B-3 | Panel | MCU input | 3/4" RMC | 3 #10 (per MCU rating) | 1 #10 | Size/OCPD per MCU nameplate (TBD) |
| M-1 | MCU | Azimuth motor | 3/4" RMC, LFMC final | 3 #12 (or VFD cable) | 1 #12 | Drive output; own conduit; length limit TBD |
| S-1 | MCU | Azimuth drive (encoder, limits, brake) | 3/4" RMC, separate | Per vendor cable | n/a | Signal/control; separate from M-1; spare conduit |

## 5. Open questions to close out (in order of impact)

1. **MCU data sheet / nameplate:** input voltage, phase, rated input current, max OCPD and fuse class, SCCR, required disconnect, and filter/cable requirements. (Replaces "motor HP/voltage/phase" as the top question.)
1a. Motor nameplate: HP, voltage, FLA, brake, heater, thermal, encoder type, and the **maximum cable length** the MCU allows to the motor.
1b. **Where does the MCU sit** (at the structure or remote/indoors)? Is it already in an enclosure rated for outdoors?
1c. Does the azimuth axis use a **slip ring or cable wrap**, and what is the **rotation range**? Who supplies the cable across the joint?
2. Source voltage, and where it comes from (existing panel, spare breaker, new feeder)?
3. Total one-way run length, including the vertical rise?
4. Marine/coastal site? Any contract requirement for PVC-coated, stainless, or nonmetallic?
5. Which UFC / NAVFAC documents does the contract name? (Check UFC 3-501-01 and the exterior distribution and lightning protection criteria.)
6. Are the 115 V loads on receptacles (GFCI rules, NEC 210.8) or hard-wired?
7. Does the structural engineer approve attaching to the structure, and where?
8. Who owns the available fault current / arc-flash data?
