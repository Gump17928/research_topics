# Home Network Plan: Switches, Decos, and Server Rack

## 1. Current understanding

- Fiber enters the house into an **ISP box**. It has several Ethernet ports, but not enough for everything.
- A **TP-Link Deco** system is the router and Wi-Fi. The main Deco connects to the ISP box today.
- About **4 wired drops** end near the ISP box.
- The **server rack is in another location**. It already has several devices and can't have its own direct runs to the house drops.
- You want a **temporary, cheap** setup and plan to sell the extra switch(es) later.
- The TL-SG108 (Amazon and Best Buy listings are the same switch) is an **unmanaged** 8-port Gigabit switch. It has no app, no settings and no loop protection.

### Rules for all plans

1. **Only one cable between any two switches.** Two cables create a loop that will bring the network down. If it happens, unplug the extra cable.
2. **Keep the router in one place.** Either the Deco is the router (router mode) or the ISP box is (Decos in AP mode). Don't make both route.
3. **Cables run max 100 m (328 ft)** for Cat5e/Cat6.
4. **Label every port and cable.**

### Open questions

- [ ] Is the ISP box a router (Wi-Fi, admin page) or just a fiber ONT?
- [ ] Is there already a cable run from the house to the rack?
- [ ] What are the "several" things in the rack: servers, or switches? How many ports do they need?

---

## 2. Plan A: Two small temporary switches (current idea)

Cost: about $30-50 total. Resale value is good.

```
ISP box
   │
   ▼
Main Deco (router mode, WAN port)
   │  (LAN port)
   ▼
Switch A: TL-SG108 (house)
   ├── Drop 1
   ├── Drop 2
   ├── Drop 3
   ├── Drop 4
   ├── (spare)
   ├── (spare)
   └── Uplink cable ─────────────►  Switch B: TL-SG108 (rack)
                                       ├── Server / device 1
                                       ├── Server / device 2
                                       ├── ...
                                       └── up to 7 devices
```

**Switch A port use:** 1 Deco, 4 drops, 1 uplink = 6 of 8 ports.
**Switch B port use:** 1 uplink, 7 free ports for devices.

**Steps**
1. Plug the main Deco's second Ethernet port into Switch A.
2. Plug the 4 drops into Switch A.
3. Run one cable from Switch A to Switch B in the rack.
4. Move the rack devices onto Switch B.
5. If any device lacks internet, reboot the Deco.

**Notes:** If the rack already has a switch with a free port, skip Switch B and run the uplink straight into it.

---

## 3. Plan B: Bare-minimum "big switch" (one switch, no rack switch)

Cost: about $25-45 for a 16-port. Use it only if the rack devices fit on a switch you already have, or if there are few of them and a single cable run is enough.

```
ISP box
   │
   ▼
Main Deco (router mode, WAN port)
   │  (LAN port)
   ▼
Big switch (16-port, house)
   ├── Drop 1
   ├── Drop 2
   ├── Drop 3
   ├── Drop 4
   ├── Other Decos (if wired)
   └── Uplink ──► rack (existing switch or a single device)
```

**Alternative layout (Decos as APs, ISP box as router):**

```
ISP box (router)
   │
   ▼
Big switch (16-port, house)
   ├── Drop 1-4
   ├── Decos (set to Access Point mode in the app)
   └── Uplink ──► rack
```

- This works only if the ISP box handles DHCP. You lose Deco router features (parental controls, QoS, HomeShield).
- Don't run the Deco in router mode behind a router ISP box, which means double NAT.

**Downside:** If the rack has several devices and no switch, one cable can serve only one device. You need a rack-side switch anyway, so Plan A or C is the more realistic version.

---

## 4. Plan C: Rack-mounted "big switch" for the rack

Cost: about $40-100, depending on port count. Use it when the rack is the main hub and you want it to look tidy.

```
ISP box
   │
   ▼
Main Deco (router mode, WAN port)
   │  (LAN port)
   ▼
Switch A: small 8-port (house, collects the 4 drops)
   │
   └── Uplink cable ─────────────►  Rack switch: 16-24 port, rack-mount
                                       ├── Server 1
                                       ├── Server 2
                                       ├── Server 3
                                       ├── ...
                                       └── Spare ports for growth
```

**Hardware to consider (unmanaged, rack-mount):** TP-Link TL-SG1016D (16-port) or TL-SG1024D (24-port).

**Notes**
- The small house switch stays, since the 4 drops end there.
- Check the rack's mounting width: these are 19-inch / 1U.
- If you want 10G or 2.5G later, get a switch with an SFP+ uplink.

---

## 5. Plan D (end game): Managed router + managed switches, Decos become APs

Cost: roughly $200-500+, depending on the ecosystem and port count. Use it when you want one place to manage routing, switching and VLANs.

The Decos stay in use: you switch them to **Access Point mode** in the Deco app and they only provide Wi-Fi. A new router takes over routing, DHCP and firewall.

```
ISP box (ONT only, or gateway in bridge/passthrough mode)
   │
   ▼
New router / gateway (managed, WAN port)
   │  (LAN / switch port)
   ▼
Managed switch (house, collects the 4 drops)
   ├── Drop 1-4
   ├── Main Deco (AP mode)
   ├── Other Decos (AP mode, wired backhaul if possible)
   └── Uplink (single cable, or SFP+ fiber/DAC) ──►  Managed rack switch (rack-mount)
                                                         ├── Server 1
                                                         ├── Server 2
                                                         └── ...
```

**Ecosystem options**

| Option | Router | Switches | Notes |
|---|---|---|---|
| TP-Link Omada | ER605 / ER7206 | SG2008, SG2016, or PoE variants | One controller (software or hardware OC200). Pairs well with your TP-Link gear. |
| Ubiquiti UniFi | Cloud Gateway / Dream Machine | USW-Lite-8, USW-16, Pro rack models | Polished dashboard. More expensive. |
| Router + managed switch (e.g. MikroTik) | Any with a good port count | Any managed | Cheapest and most flexible, but with the most manual setup. |

Some of these devices are a router and switch in one box (UniFi Dream Machine, some Omada gateways), with only 4-8 ports. You would still add a switch for the drops and the rack.

**Before you buy**
- Choose the ecosystem first. Decos can't be adopted by Omada or UniFi, so they stay in the Deco app as APs and the Wi-Fi is managed separately from the rest.
- **Decos in AP mode lose their router features:** parental controls, HomeShield, QoS. The new router replaces them.
- Check whether your ISP box is an **ONT only** (then the new router connects directly) or a **gateway** (then put it in bridge/passthrough mode, or you get double NAT).
- If you want PoE for cameras or APs, buy PoE switches. Most Decos are powered by their own adapters.
- Pick a rack uplink speed. 1G is fine to start. For heavy server traffic, choose switches with 2.5G or 10G SFP+ ports.

**Migration steps (keeping the network down for a short time)**
1. Buy and set up the new router, switches and controller on the bench first.
2. In the Deco app, change the main Deco and the others to **Access Point mode**.
3. Connect the ISP box to the new router's WAN port. Reboot the ISP box if the link doesn't come up.
4. Connect the house switch to the router, then plug in the 4 drops and the Decos.
5. Run one cable from the house switch to the rack switch and move the rack devices over.
6. Enable loop protection (STP/RSTP) on the managed switches, then plan VLANs if wanted (e.g. servers, IoT, guests).
7. Sell the two TL-SG108 units from Plan A.

**Why it's the end state:** Routing, switching and VLANs sit in one place, loops are protected by STP, and you get per-port stats and monitoring. You keep the Decos you already own as APs.

---

## 6. Comparison

| | Plan A | Plan B | Plan C | Plan D |
|---|---|---|---|---|
| Devices | 2 × 8-port | 1 × 16-port | 1 small + 1 rack-mount | Router + 2 managed switches |
| Cost | ~$30-50 | ~$25-45 | ~$60-150 | ~$200-500+ |
| Fits the rack's many devices | Yes (7 ports) | Only with an existing rack switch | Yes (16-24 ports) | Yes |
| Management | None | None | None | Single dashboard |
| Loop protection | No | No | No | Yes |
| Deco role | Router + Wi-Fi | Router + Wi-Fi (or APs) | Router + Wi-Fi | APs only |
| Temporary / resalable | Easy | Easy | Medium | Long-term |
| Best for | Cheap start | Few rack devices | Tidy rack | End game |

**Recommendation:** Start with Plan A now. Move to Plan D when you want VLANs, monitoring and one dashboard. The small switches can be sold or reused for the desk or the TV.

---

## 7. Suggested path

```
Plan A (now, cheap)
   │
   ├─► Plan C (rack gets bigger)
   │
   └─► Plan D (end game: managed router + switches, Decos as APs)
```
