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

### 0A. Sizing basis for initial review (rev 3: 5 HP motor, other loads under 5 A)

These values are **placeholders chosen so the schedules in Section 4 are complete**. Every row marked ASSUMED must be replaced with nameplate/data-sheet values. The design is conservative: if the real values are lower, the sizes below still work. If they are higher, re-run the numbers.

| # | Item | Assumed value | Status | Basis / how it was derived |
|---|---|---|---|---|
| B1 | Source system | 208Y/120 V, 3-phase, 4-wire + EGC, 60 Hz | ASSUMED | Common outdoor-structure source. Matches A4 |
| B2 | Azimuth motor | **5 HP, 208 V, 3-phase, FLA 16.7 A** | ASSUMED (HP not confirmed) | NEC Table 430.250, 5 HP at 208 V |
| B3 | MCU input | 208 V, 3-phase, **18 A rated input**, **30 A max OCPD** | ASSUMED | Typical 5 HP drive input is about 1.05 to 1.15 times motor FLA. Replace with MCU nameplate |
| B4 | Other loads | **2 loads, 115 V, 1-phase, 5 A each** (the "under 5 A" limit, used as the design value) | ASSUMED | Hard-wired (not receptacles), treated as **continuous** (125%) to stay conservative |
| B5 | Feeder one-way length | **150 ft** (includes about 40 ft of vertical rise) | ASSUMED | Placeholder from the earlier worked example. Controls voltage drop |
| B6 | Panel location | **PNL-OA at the structure**, within 25 ft of the MCU | ASSUMED | Keeps the branch runs short |
| B7 | MCU location | **At the structure beside the drive**, in a NEMA 4X enclosure | ASSUMED (A3c) | Makes the MCU-to-motor cable about 25 ft (limit assumed 50 ft) |
| B8 | Branch lengths | B-1 and B-2: 50 ft. B-3: 25 ft. M-1: 25 ft | ASSUMED | Voltage drop and conduit lengths |
| B9 | Feedback and control | Encoder (shielded), 2 limit switches, motor thermal (PTC). **No holding brake** | ASSUMED | Replace with the vendor's cable list. A brake adds a 2-conductor power circuit |
| B10 | Rotating joint | **Cable wrap/loop**, rotation not more than 360 degrees, no slip ring | ASSUMED | If a slip ring is used, the vendor supplies it and the wiring changes at that point |
| B11 | Ambient | **40 deg C** design ambient, outdoor, marine air | ASSUMED | NEC Table 310.15(B)(1) correction factor of 0.91 on the 90 deg C column |
| B12 | Available fault current | **10 kA or less** at PNL-OA | ASSUMED | Breakers and the panel are specified at 10 kAIC minimum. MCU SCCR and fuse type must be confirmed |
| B13 | Conductors | Copper, **THWN-2**, stranded, minimum #12 | ASSUMED (Navy practice **[VERIFY]**) | Matches Section 3.1 |
| B14 | Raceway | **RMC** exposed on the structure, **LFMC** for final connections, minimum 3/4 in | ASSUMED (marine: confirm PVC-coated or stainless) | Matches Section 2 |
| B15 | Disconnects | Lockable, within sight of the MCU. No separate motor disconnect if the MCU disconnect is in sight of the motor (not more than 50 ft) | ASSUMED **[VERIFY 430.102]** | Add a motor disconnect if line of sight fails |
| B16 | Voltage drop limits | 3% feeder, 3% branch, **5% total** | ASSUMED (Navy may be stricter **[VERIFY]**) | Matches Section 3.2 |

If the motor turns out to be **460 V** instead, the 5 HP FLA is 7.6 A. The feeder would then need a **480 V source or a transformer**, and every size in Section 4.3 drops to #12 to #10 or smaller. Treat that as a rev change, not a tweak.

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

### 3.4 Worked example (uses the Section 0A assumptions: 5 HP motor, 5 A loads)

**Loads:** MCU input 18 A (208 V, 3-ph, B3), two 115 V loads at 5 A each (B4), motor FLA 16.7 A (B2).

1. **MCU branch (B-3):** 18 x 1.25 = **22.5 A** (NEC 430.122). #10 Cu THWN-2: 90 deg C ampacity 40 A x 0.91 (40 deg C ambient) = 36.4 A, and the 75 deg C terminal limit is 35 A. Both are above 22.5 A. OCPD: **30 A, 3-pole** (the assumed MCU maximum). EGC **#10** (Table 250.122, 30 A). #12 would meet ampacity, but #10 gives margin and avoids a small-conductor question.
2. **115 V branches (B-1, B-2):** 5 x 1.25 = 6.25 A. **20 A, 1-pole breaker, 2 #12 + #12 EGC**. Voltage drop at 50 ft: 2 x 5 x 2.0 x 50 / 1000 = 1.0 V = **0.8% of 120 V**.
3. **Feeder (F-1):** 22.5 A (MCU at 125%) + 6.25 + 6.25 (loads at 125%) = **35 A** (conservative, not phase-balanced).
   - Breaker: **40 A, 3-pole**. Check 430.62: largest branch device (30 A) + other loads (5 + 5 A) = 40 A, so 40 A is permitted.
   - Conductors: ampacity-wise #8 would work (50 A at 75 deg C). **Voltage drop controls**: at 150 ft one-way, 35 A, 3-ph, #8: 1.732 x 35 x 0.78 x 150 / 1000 = 7.1 V = **3.4%** (over the 3% target). **#6**: 1.732 x 35 x 0.49 x 150 / 1000 = 4.5 V = **2.1%**. Use **4 #6 THWN-2** (3 phases + neutral).
   - Derating check on #6: 75 A (90 deg C) x 0.91 x 0.80 (4 conductors counted) = 54.6 A, above 35 A. The 75 deg C terminal limit is 65 A, which is also above 35 A.
   - EGC: **#8**. A 40 A device needs #10 (Table 250.122), and increasing the phase conductors from #8 to #6 requires the EGC to grow in proportion (250.122(B)), which gives #8.
   - Conduit fill: 4 x #6 (0.0507 in2) + 1 x #8 (0.0366 in2) = **0.239 in2**. 1 in RMC (0.355 in2 at 40%) passes at about 27%. Use **1-1/4 in RMC** (0.610 in2 at 40%, about 16% fill) for pulling room up the vertical rise and spare capacity.
4. **MCU-to-motor (M-1):** 16.7 x 1.25 = **20.9 A** (NEC 430.22). #12 meets ampacity (25 A at 75 deg C), but the drive output is a harsh environment and the manufacturer's cable may be #10. Use **#10 VFD-rated shielded cable (3 power + 3 symmetrical grounds)**. Voltage drop at 25 ft: 1.732 x 16.7 x 1.2 x 25 / 1000 = 0.9 V = **0.4%**. Shielded VFD cable has a larger outside diameter (assume 0.8 in), so use **1-1/4 in RMC** (single cable at up to 53% fill). Verify the real cable OD.
5. **Total voltage drop (worst case):** feeder 2.1% + MCU branch (25 ft, #10, 18 A: 1.732 x 18 x 1.2 x 25 / 1000 = 0.9 V = 0.4%) = **2.5% of 208 V**. Under the 5% total. This is conservative, because the feeder is calculated at 35 A and the real running current is about 18 A.
6. **Connected load:** MCU 18 A x 208 V x 1.732 = **6.5 kVA** + 2 x 0.6 kVA = **7.7 kVA** (21 A at 208 V). The 40 A feeder has about 47% headroom for growth.

> Re-run these numbers if the motor is not 5 HP, if the MCU input is above 18 A, if the run is longer than 150 ft, or if the supply voltage differs.

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

Place this on the sheet. The bracketed values are the Section 0A assumptions. Replace each with nameplate data as it is confirmed, and delete "ASSUMED" from the item once verified.

> **ASSUMPTIONS (INITIAL, FOR REVIEW):**
> 1. Source is **208Y/120 V, 3-phase, 4-wire**, available at [existing panel / spare breaker, TBD]. Available fault current is assumed **10 kA or less** at the structure panel (PNL-OA).
> 2. Azimuth drive MCU (MCU-1) is [model TBD], **208 V, 3-phase, 18 A rated input, 30 A maximum OCPD**. MCU is located **at the structure beside the drive**, in a NEMA 4X enclosure.
> 3. Azimuth motor (M-1) is **5 HP, 208 V, 3-phase, 16.7 A FLA** (NEC Table 430.250), located **about 25 ft** from the MCU. No holding brake. Feedback is a shielded encoder, two limit switches, and a motor thermal sensor.
> 4. Two 115 VAC, single-phase loads (L-1, L-2) of **5 A or less each**, hard-wired (not receptacles), treated as continuous.
> 5. One-way feeder length is **150 ft** (including about 40 ft of vertical rise). Branch lengths: B-1 and B-2 50 ft, B-3 25 ft.
> 6. Design ambient is **40 deg C**, outdoor, marine atmosphere. Copper THWN-2 conductors in RMC, LFMC at final connections.
> 7. Voltage drop is limited to **3% on the feeder and 5% overall**.
> 8. Azimuth rotation is not more than **360 degrees**, using a cable wrap/loop (no slip ring).
>
> **All sizes shown are based on these assumptions. If any assumption changes, conductor, conduit, enclosure, and protective device sizes shall be re-verified before ordering or installation.**

### 4.3 Suggested Revit schedules (complete set for initial review)

All values come from Section 0A and the Section 3.4 calculations. Items marked **TBD** need vendor or site data. Keep the tags below identical on the plan, the schedules, and the ACADE diagram.

#### 4.3.1 Equipment tags and legend

| Tag | Description | Rating (assumed) | Location | Enclosure |
|---|---|---|---|---|
| SRC | Existing source panel or switchboard (TBD) | 208Y/120 V, 3-ph, 4-wire | Existing building (TBD) | Existing |
| F-1-DS | Feeder breaker at the source | 40 A, 3-pole, 10 kAIC minimum | At SRC | Existing |
| PNL-OA | Outdoor panelboard | 208Y/120 V, 3-ph, 4-wire, 100 A bus, 40 A main breaker, 12 spaces, 10 kAIC | At the structure | NEMA 4X (stainless or FRP) |
| DS-1 | Disconnect for MCU-1 | 3-pole, 30 A, lockable (fused only if the MCU requires it) | In sight of MCU-1 | NEMA 4X |
| SPD-1 | Surge protective device at PNL-OA | 3-ph, 208Y/120 V, Type 1 or 2 (TBD per lightning criteria) | At PNL-OA | NEMA 4X |
| MCU-1 | Azimuth motor controller unit | 208 V, 3-ph, 18 A input, 30 A max OCPD | At the structure beside the drive | NEMA 4X |
| M-1 | Azimuth drive motor | 5 HP, 208 V, 3-ph, 16.7 A FLA | On the structure | Per motor |
| L-1 | 115 V load 1 | 115 V, 1-ph, 5 A or less | TBD | Per load |
| L-2 | 115 V load 2 | 115 V, 1-ph, 5 A or less | TBD | Per load |
| JB-1 | Junction/pull box at the rotating joint | Sized for cable wrap (TBD) | At the azimuth axis | NEMA 4X |
| GND-1 | Structure grounding connection | Per structure grounding drawings | At PNL-OA and MCU-1 | N/A |

#### 4.3.2 Circuit schedule (feeder, branch, drive output, signal)

| Tag | From | To | Voltage / phase | Load (A) | OCPD | Phase conductors | Neutral | EGC | Length (ft) | Voltage drop | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|
| F-1 | SRC | PNL-OA | 208Y/120 V, 3-ph | 35 (calc, 125% basis) | 40 A / 3P | 3 #6 | 1 #6 | 1 #8 | 150 | 2.1% | Voltage drop controls the size (#8 would be 3.4%). Verify the length in the field |
| B-1 | PNL-OA ckt 1 | L-1 | 120 V, 1-ph | 5 | 20 A / 1P | 1 #12 | 1 #12 | 1 #12 | 50 | 0.8% | On phase A. Hard-wired |
| B-2 | PNL-OA ckt 3 | L-2 | 120 V, 1-ph | 5 | 20 A / 1P | 1 #12 | 1 #12 | 1 #12 | 50 | 0.8% | On phase B. Hard-wired |
| B-3 | PNL-OA ckt 2-4-6 | DS-1, then MCU-1 | 208 V, 3-ph | 18 (22.5 at 125%) | 30 A / 3P | 3 #10 | none | 1 #10 | 25 | 0.4% | Size and OCPD to be set from the MCU nameplate (TBD). No neutral to the MCU |
| M-1 | MCU-1 | Azimuth motor | 208 V, 3-ph (drive output) | 16.7 (20.9 at 125%) | MCU provides overload (430.126) | 3 #10 (shielded VFD cable) | none | 3 symmetrical grounds (in the cable) | 25 | 0.4% | Own raceway. Use the manufacturer's cable. Verify max length (50 ft assumed) |
| S-1 | MCU-1 | Encoder, limit switches, thermal sensor | Low voltage | 1 or less | n/a | Vendor shielded multi-conductor cable | n/a | Drain/shield per vendor | 25 | n/a | Separate raceway from M-1. Terminate shields 360 degrees |
| S-2 | MCU-1 | Control/comms (E-stop, enable, comms) | Low voltage (24 V DC typical, TBD) | 1 or less | n/a | Per vendor / controls interface | n/a | Per vendor | TBD | n/a | May share a raceway with S-1 only if the manufacturer allows. Control source TBD |
| G-1 | PNL-OA ground bar | Structure ground | n/a | n/a | n/a | n/a | n/a | #6 Cu bare or insulated (TBD per structure grounding design) | 25 | n/a | Bond MCU-1, M-1 frame, PNL-OA, and conduits. Size per the structure grounding/lightning drawings (TBD) |

#### 4.3.3 Raceway schedule (conduit and fill)

| Conduit tag | Carries | Conduit | Wires inside | Fill (40% limit) | Termination | Notes |
|---|---|---|---|---|---|---|
| C-1 | F-1 | **1-1/4" RMC** (PVC-coated or stainless if marine **[VERIFY]**) | 4 #6 + 1 #8 THWN-2 | 0.239 in2, about 16% | Threaded, listed wet-location fittings | Exposed on the structure. Expansion fittings at structural joints, drain fittings at low points, vertical supports per 300.19 |
| C-2 | B-1 | **3/4" RMC**, 3/4" LFMC final to L-1 | 2 #12 + 1 #12 THWN-2 | 0.040 in2, about 7.5% | Listed fittings | May share C-2 with B-2 if the field prefers one conduit (4 #12 + 1 #12, about 12.5% fill) |
| C-3 | B-2 | **3/4" RMC**, 3/4" LFMC final to L-2 | 2 #12 + 1 #12 THWN-2 | 0.040 in2, about 7.5% | Listed fittings | Same as C-2 |
| C-4 | B-3 | **3/4" RMC** | 3 #10 + 1 #10 THWN-2 | 0.084 in2, about 16% | PNL-OA to DS-1 to MCU-1 | Keep separate from M-1 and S-1 |
| C-5 | M-1 | **1-1/4" RMC**, LFMC final to the motor (listed fittings) | 1 shielded VFD cable, 3 #10 + 3 grounds (assumed OD 0.8 in) | Single cable at 53% fill limit | MCU-1 to M-1 | Own raceway. Maintain shield and ground continuity. Cable wrap across the rotating joint |
| C-6 | S-1 (and S-2 if allowed) | **3/4" RMC** | Vendor encoder/limit/thermal cable | Per cable OD | MCU-1 to the drive | Cross power conduits at right angles. No shared raceway with C-4 or C-5 |
| C-7 | Spare | **3/4" RMC**, capped | Pull string only | 0% | PNL-OA to JB-1 (or the nearest structure point) | Spare for future access. Label "SPARE" at both ends |

All RMC to be **stainless or PVC-coated where marine exposure requires**. Minimum conduit size 3/4".

#### 4.3.4 Panel schedule: PNL-OA (208Y/120 V, 3-ph, 4-wire, 100 A bus, 40 A main, 10 kAIC)

| Ckt | Description | Load (VA) | Phase | Breaker | Wire | Ckt | Description | Load (VA) | Phase | Breaker | Wire |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | L-1 (B-1) | 600 | A | 20 A / 1P | #12 | 2 | MCU-1 (B-3) | 2,161 | A | 30 A / 3P | #10 |
| 3 | L-2 (B-2) | 600 | B | 20 A / 1P | #12 | 4 | (MCU-1) | 2,161 | B | (3P) | #10 |
| 5 | SPARE | 0 | C | 20 A / 1P | n/a | 6 | (MCU-1) | 2,161 | C | (3P) | #10 |
| 7 | SPACE | | A | n/a | | 8 | SPACE | | A | n/a | |
| 9 | SPACE | | B | n/a | | 10 | SPACE | | B | n/a | |
| 11 | SPACE | | C | n/a | | 12 | SPACE | | C | n/a | |

| Phase | Connected load (VA) | Current (A) |
|---|---|---|
| A | 2,761 | about 23 at 120 V (MCU phase current 18 A, plus 5 A) |
| B | 2,761 | about 23 |
| C | 2,161 | 18 |
| **Total** | **7,683 VA** | **21.3 A at 208 V (balanced equivalent)** |

Feeder design current: **35 A** (22.5 A MCU at 125% + 6.25 + 6.25 for 115 V loads at 125%). Main breaker 40 A.

#### 4.3.5 Voltage-drop and sizing summary

| Run | Current used | Length (ft) | Conductor | Voltage drop (V) | Voltage drop (%) | Limit |
|---|---|---|---|---|---|---|
| F-1 | 35 A | 150 | #6 | 4.5 | 2.1% | 3% |
| B-1 / B-2 | 5 A | 50 | #12 | 1.0 | 0.8% | 3% |
| B-3 | 18 A | 25 | #10 | 0.9 | 0.4% | 3% |
| M-1 | 16.7 A | 25 | #10 | 0.9 | 0.4% | Vendor limit |
| **Worst path total (F-1 + B-3)** | | | | | **2.5%** | **5%** |

#### 4.3.6 Keynote legend (for the plan view)

| Key | Note |
|---|---|
| 1 | Provide outdoor panelboard PNL-OA, NEMA 4X, mounted on the structure per structural approval. Label per Note 11. |
| 2 | Route feeder F-1 in 1-1/4" RMC up the structure. Provide expansion fittings at joints and a drain at the low point. |
| 3 | Provide lockable disconnect DS-1 in sight of MCU-1. |
| 4 | Provide MCU-1 per vendor data. Size supply, OCPD, and cable per the MCU nameplate. |
| 5 | Drive output M-1 in its own conduit. Shielded VFD-rated cable. Maintain shield and ground continuity. |
| 6 | Signal and control cable S-1 in a separate conduit. Cross power at right angles. |
| 7 | Cable wrap/loop at the azimuth joint (JB-1). Size for full travel. Verify at both stops before energizing. |
| 8 | Spare 3/4" RMC with pull string. Cap and label. |
| 9 | Bond conduits, boxes, MCU-1, and the motor frame to the structure grounding (GND-1). |

#### 4.3.7 How to build these in Revit

1. **Equipment schedule:** create a schedule of the **Electrical Equipment** category. Fields: Mark, Description, Rated voltage, Phase, Rated current, Location, Enclosure (shared parameter), Notes.
2. **Circuit schedule:** create a schedule of the **Electrical Circuits** category. Fields: Circuit Number, Load Name, Voltage, Phase, Apparent Load/Current, Rating (breaker), Wire Size, Length, Voltage Drop, Notes. If circuits are not modeled, use a **Drafting View with a table** or a **Revit schedule with manual rows** and keep the tags identical to the plan.
3. **Raceway schedule:** create a **Conduits** schedule. Fields: Mark (C-1 to C-7), Type, Size, Length, From, To, plus shared parameters for Wires Inside and Fill percent.
4. **Panel schedule:** use **Create Panel Schedule** on PNL-OA (or a drafting-view table if electrical connectors are not modeled).
5. **Keynotes:** add the keynote legend as a **Keynote Legend** and tag with **Keynote Tags**.
6. **Sheet layout:** E-101 holds the plan, general notes, assumption note (4.2), keynotes, and the equipment and raceway schedules. E-201 (ACADE) holds the one-line diagram and circuit schedule. Put the voltage-drop summary on E-201 or keep it in the calculation package.

#### 4.3.8 Items still marked TBD in the schedules

- MCU model, input rating, maximum OCPD, fuse class, SCCR, and cable requirements.
- Motor HP and voltage (5 HP at 208 V assumed). A holding brake adds a power circuit.
- Source panel, available breaker space, and fault current.
- Field-verified run lengths (150 ft feeder and 25/50 ft branches).
- Site marine exposure (PVC-coated or stainless RMC) and the contract's UFC/NAVFAC criteria.
- Rotating-joint method (cable wrap or slip ring) and rotation range.
- SPD requirement from the lightning protection criteria.
- Ground conductor size from the structure grounding design.

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
