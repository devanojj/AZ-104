Replication creates copies of storage data to provide **durability and availability** during failures.

**Exam Fact**

- **LRS → ZRS → GRS → GZRS** = increasing protection
- **RA-** = read access to the secondary region

---

#### LRS — Locally Redundant Storage

- **3 copies** within a single data center
- **Lowest cost**
- Least protection against data-center disasters
- Suitable when:
    - Data can be reconstructed
    - Data loss isn't critical
    - Governance requires data to stay in one location

**Protects against:** Node/hardware failure  
**Doesn't protect against:** Data-center or regional failure

---

#### ZRS — Zone-Redundant Storage

- **3 synchronous copies** across **3 availability zones**
- Same Azure region
- Protects against an entire **availability zone** failure
- Good performance and low latency
- Not available in every region

**Protects against:** Node + zone failure  
**Doesn't protect against:** Regional failure

---

#### GRS — Geo-Redundant Storage

- Replicates data to a **secondary region**
- Secondary region is geographically distant
- Replication to secondary is **asynchronous**
- Secondary data is **not readable** unless Microsoft initiates failover
- Designed for **regional disaster recovery**
- **16 9's durability**

**Architecture:**

```
Primary Region → LRS
       ↓
Secondary Region → LRS
```

---

#### RA-GRS — Read-Access GRS

- Same replication as **GRS**
- Can **read from secondary region at any time**
- Useful during primary-region outages

**Exam Tip**

- **GRS = secondary region, no normal read access**
- **RA-GRS = secondary region + read access**

---

#### GZRS — Geo-Zone-Redundant Storage

**Definition:** Combines **ZRS + GRS**.

- **3 copies across availability zones** in primary region
- Also replicated to a **secondary region**
- Protects against:
    - Zone failures
    - Regional disasters
- **16 9's durability**
- Microsoft recommends GZRS for high availability + disaster recovery

**Architecture:**

```
Primary Region
 ├─ Zone 1
 ├─ Zone 2
 └─ Zone 3
       ↓
Secondary Region
```

---

#### RA-GZRS

- Same as **GZRS**
- Adds **read access to secondary region**
- Best when secondary-region reads are required during regional disasters

---

#### Replication Comparison

|Type|Zones|Secondary Region|Read Secondary|
|---|---|---|---|
|**LRS**|❌|❌|❌|
|**ZRS**|✅ 3|❌|❌|
|**GRS**|❌|✅|❌|
|**RA-GRS**|❌|✅|✅|
|**GZRS**|✅ 3|✅|❌|
|**RA-GZRS**|✅ 3|✅|✅|

---

#### Failure Protection

|Failure|Supported|
|---|---|
|Node failure|**All**|
|Entire data center/zone|**ZRS, GRS, RA-GRS, GZRS, RA-GZRS**|
|Regional outage|**GRS, RA-GRS, GZRS, RA-GZRS**|
|Read during regional outage|**RA-GRS, RA-GZRS**|

---

#### Exam Facts

- **LRS** → 3 copies, single data center
- **ZRS** → 3 copies, 3 availability zones
- **GRS** → secondary region, asynchronous replication
- **RA-GRS** → GRS + read secondary
- **GZRS** → ZRS + GRS
- **RA-GZRS** → GZRS + read secondary
- **RA = Read Access**
- **GRS/GZRS secondary replication is asynchronous**
- **16 9's durability** → GRS, GZRS and their RA variants