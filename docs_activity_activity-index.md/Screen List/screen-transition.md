# Screen Transition Diagram: MySen Platform

## 1. Visitor / GenZ Customer Flow

```mermaid
flowchart LR
  entry(["Entry: Opens App"])
  S00_01["S-F00-01<br>Sign In"]
  S01_01["S-F01-01<br>Profile"]
  S02_01["S-F02-01<br>E-Ticket Catalogue"]
  S02_02["S-F02-02<br>E-Ticket Checkout"]
  S02_03["S-F02-03<br>Ticket Wallet"]
  S03_01["S-F03-01<br>Interactive 2.5D Map"]
  S04_01["S-F04-01<br>Quest Hub & QR Scanner"]
  S04_02["S-F04-02<br>Storybook Reader"]
  S04_03["S-F04-03<br>Check-in Streak"]
  S04_04["S-F04-04<br>Voucher Exchange"]
  S05_01["S-F05-01<br>Social Gallery"]
  pushStreak(["Push: Streak Reminder"])

  entry --> S00_01
  pushStreak --> S04_03

  S00_01 -->|"signed in"| S04_03
  S04_03 -->|"taps Profile"| S01_01
  S01_01 -->|"back"| S04_03
  S01_01 -->|"confirms Log Out"| S00_01

  S04_03 -->|"taps Tickets"| S02_01
  S02_01 -->|"selects tickets & continues"| S02_02
  S02_02 -->|"payment success"| S02_03
  S02_03 -->|"browse tickets"| S02_01

  S04_03 -->|"taps Open Park Map"| S03_01
  S04_03 -->|"taps Scan to Check In"| S04_01
  S04_01 -->|"scans valid QR"| S04_02
  S04_02 -->|"taps Continue Quest"| S04_01
  S04_01 -->|"[check-in confirmed]"| S04_03

  S04_03 -->|"taps Redeem Coins"| S04_04
  S04_04 -->|"back / taps coin balance"| S04_03

  S04_03 -->|"taps Social Gallery"| S05_01
  S05_01 -->|"taps Profile Avatar"| S01_01
```

---

## 2. Partner / Commercial Flow

```mermaid
flowchart LR
  entry(["Entry: Opens App"])
  S00_01["S-F00-01<br>Sign In"]
  S05_02["S-F05-02<br>Partner Rental & Booking"]

  entry --> S00_01
  S00_01 -->|"[signed in as Partner]"| S05_02
  S05_02 -->|"confirms Log Out"| S00_01
```

---

## 3. Staff & Admin Flow

```mermaid
flowchart LR
  entry(["Entry: Opens Admin Portal"])
  S00_01["S-F00-01<br>Sign In"]
  S06_01["S-F06-01<br>Admin Dashboard"]

  entry --> S00_01
  S00_01 -->|"[signed in as Staff/Admin]"| S06_01
  S06_01 -->|"confirms Log Out"| S00_01
```

---

## 4. Transition Mapping Table

| Source Screen | Trigger / User Action | Target Screen |
|---|---|---|
| `S-F00-01` | Sign in succeeds (Role-based) | `S-F04-03` (Visitor) / `S-F06-01` (Staff) |
| `S-F04-03` | Taps Tickets tab | `S-F02-01` (E-Ticket Catalogue) |
| `S-F02-01` | Selects ticket quantity & taps Continue | `S-F02-02` (E-Ticket Checkout) |
| `S-F02-02` | Verified payment success | `S-F02-03` (Ticket Wallet) |
| `S-F04-03` | Taps "Scan to Check In" | `S-F04-01` (Quest Hub & QR Scanner) |
| `S-F04-01` | Scans valid checkpoint QR | `S-F04-02` (Storybook Reader) |
| `S-F04-03` | Taps "Redeem Coins" | `S-F04-04` (Voucher Exchange) |
| `S-F04-03` | Taps "Open Park Map" | `S-F03-01` (Interactive 2.5D Map) |
| `S-F04-03` | Taps Social Gallery tab | `S-F05-01` (Social Feed) |
| `S-F00-01` | Sign in as Partner | `S-F05-02` (Partner Rental & Booking) |
