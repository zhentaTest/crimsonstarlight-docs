# # SRPG 아이템 디자인 문서 v1.1
## 버전 이력
| **버전** | **날짜** | **변경 내용** |
|:-:|:-:|:-:|
| v1.0 | 2025-05-08 | • 최초 문서 작성<br>• 무기 시스템 정의<br>• 방어구 시스템 정의<br>• 소모성 아이템 정의<br>• 상태이상 아이템 체계화<br>• 아이템 데이터 구조 설계 |
| v1.1 | 2025-05-08 | • 무기 피해 계산 시점 명확화<br>• 통합 아이템 데이터 구조 설계<br>• 직업별 장비 제한 추가<br>• 아이템 스택 제한 설정<br>• 액션 소모 규칙 추가<br>• 질문 답변 반영 |
## 1. 개요
본 문서는 SRPG 게임의 아이템 시스템에 관한 핵심 규칙을 정의합니다. 게임 내 모든 장비와 소모성 아이템의 특성, 효과, 데이터 구조를 명시하며, 기존 전투 시스템 및 상태이상 시스템과의 연계를 구체화합니다.
### 목적 및 범위
* 게임 내 모든 아이템 종류와 속성 정의
* 장비형 아이템과 소모성 아이템의 구분 및 특성 명시
* 아이템 효과와 전투 시스템 간의 상호작용 정립
* 아이템 관련 데이터 구조 설계

⠀2. 아이템 시스템 개요
### 2.1 아이템 카테고리
* **장비형 아이템**: 캐릭터가 착용하여 지속적인 효과를 제공
  * 무기(Weapon): 공격력, 명중률, 치명타율 등 영향
  * 방어구(Armor): 방어력, 마법 저항력 증가
  * 액세서리(Accessory): 다양한 추가 효과 제공
* **소모성 아이템**: 전투 중 또는 맵에서 사용되어 일시적 효과 발생
  * 회복 아이템: HP, 상태이상 치료 등
  * 상태이상 유발 아이템: 적에게 상태이상 부여
  * 버프/강화 아이템: 일시적 능력치 상승
* **특수 아이템**: 전략 모드에서만 사용되는 아이템
  * 퀘스트 아이템: 특정 스토리/퀘스트 진행에 필요
  * 키 아이템: 특정 지역 접근 등에 필요

⠀2.2 아이템 등급 체계
| **등급** | **색상** | **특징** | **출현 확률** |
|:-:|:-:|:-:|:-:|
| 일반(Common) | 흰색 | 기본 효과, 특성 없음 | 60% |
| 고급(Uncommon) | 녹색 | 기본 효과 + 추가 능력치 1개 | 25% |
| 희귀(Rare) | 파란색 | 강화된 효과 + 추가 능력치 1-2개 | 10% |
| 영웅(Epic) | 보라색 | 강화된 효과 + 특수 능력 1개 | 4% |
| 전설(Legendary) | 주황색 | 고유 특성 + 특수 능력 1-2개 | 1% |
### 2.3 장비 부위 제한
* **무기(Weapon)**: 1개 장착 가능
* **투구(Headgear)**: 1개 장착 가능
* **갑옷(Armor)**: 1개 장착 가능
* **반지(Ring)**: 2개 장착 가능
* **목걸이(Necklace)**: 1개 장착 가능
* **주의**: 일부 양손 무기 장착 시 다른 장비 제약 가능성 있음

⠀3. 무기 시스템
### 3.1 무기 분류 체계
* **무기 유형**:
  * 한손 무기(One-Handed)
  * 양손 무기(Two-Handed)
  * 마법 무기(Magic): 셉터, 마법봉, 지팡이 등
* **공격 속성**:
  * 물리 공격(참격, 관통, 타격)
  * 마법 공격(불, 물, 번개, 대지, 어둠, 빛)
* **공격 범위**:
  * 근접(1칸)
  * 원거리(2-3칸)
  * 장거리(3-6칸)
* **무기 피해 계산**:
  * 표기된 주사위 값(예: 1d6)은 전투 시작 시 한 번 굴려 결정
  * 전투 중 동일 값 유지

⠀3.2 한손 무기 목록
| **무기** | **피해** | **유형** | **범위** | **특성** | **패널티** |
|---|:-:|:-:|:-:|---|:-:|
| 단검 (Dagger) | 1d4 | 관통 | 1 | 명중률 +10%, 공격 속도 +10% | - |
| 숏소드 (Shortsword) | 1d6 | 관통 | 1 | 명중률 +10% | - |
| 시미터 (Scimitar) | 1d6 | 참격 | 1 | 공격 속도 +10% | - |
| 레이피어 (Rapier) | 1d8 | 관통 | 1 | 치명타 확률 +5% | - |
| 롱소드 (Longsword) | 1d8 | 참격 | 1 |  | - |
| 메이스 (Mace) | 1d6 | 타격 | 1 | 기절(마비 효과) +5% | 1 |
| 모닝스타 (Morningstar) | 1d8 | 관통 | 1 | 기절(마비 효과) +10% | 1 |
| 핸드액스 (Handaxe) | 1d6 | 참격 | 1 |  | - |
| 라이트해머 (Light Hammer) | 1d4 | 타격 | 1 | 기절(마비 효과) +10% | - |
| 낫 (Sickle) | 1d4 | 참격 | 1 | 출혈 +5% | - |
| 곤봉 (Club) | 1d4 | 타격 | 1 | 기절(마비 효과) +10% | 1 |
### 3.3 양손 무기 목록
| **무기** | **피해** | **유형** | **범위** | **특성** | **패널티** |
|---|:-:|:-:|:-:|---|:-:|
| 그레이트소드 (Greatsword) | 2d6 | 참격 | 1 | 명중률 -10% | 3 |
| 그레이트액스 (Greataxe) | 1d12 | 참격 | 1 | 명중률 -5% | 3 |
| 할버드 (Halberd) | 1d10 | 참격 | 1 |  | 2 |
| 파이크 (Pike) | 1d10 | 관통 | 1 |  | 2 |
| 쿼터스태프 (Quarterstaff) | 1d8 | 타격 | 1 | 기절(마비 효과) +10% | 1 |
| 랜스 (Lance) | 1d12 | 관통 | 1 | 출혈 +5% | 3 |
| 롱보우 (Longbow) | 1d8 | 관통 | 2 |  | 2 |
| 쇼트보우 (Shortbow) | 1d6 | 관통 | 2 |  | 1 |
| 헤비 크로스보우 (Heavy Crossbow) | 1d10 | 관통 | 2 |  | 3 |
| 배틀액스 (Battleaxe) | 1d10 | 참격 | 1 |  | 2 |
| 워해머 (Warhammer) | 1d10 | 타격 | 1 |  | 2 |
| 창 (Spear) | 1d8 | 관통 | 1 |  | 1 |
| 삼지창 (Trident) | 1d8 | 관통 | 1 |  | 1 |
### 3.4 마법 무기 목록
| **무기** | **피해** | **유형** | **범위** | **특성** | **패널티** |
|---|:-:|:-:|:-:|---|:-:|
| 셉터 (Scepter) | - | - | - | 기적 효과치 +1 | - |
| 마법봉 (Wand) | - | - | - | 마법 효과 +1 | - |
| 지팡이 (Staff) | - | - | - | 마법 효과 +2 | DEX -2, MOV -1 |
| 원소 지팡이 (Elemental Staff) | - | 속성별 | 2 | 속성 마법 효과 +3 | DEX -2, MOV -1 |
| 구슬 완드 (Orb Wand) | - | 마법 | 2 | 지정 속성 마법 효과 +2 | - |
### 3.5 무기 특성 및 능력치 영향
* **무기 무게 패널티**: 무기의 패널티 수치만큼 공격 속도(AS) 감소
* **공격력 계산**: ATK = STR + weapon.might (피해 주사위 굴림 결과)
* **마법 공격력 계산**: MATK = INT + (weapon.might + 마법 효과 증가치)
* **명중률 계산**: Hit = weapon.hit + (DEX × 2) + floor(LUK / 2)
* **치명타율 계산**: Crit = weapon.crit + floor(DEX / 2)

⠀4. 방어구 시스템
### 4.1 투구 (Headgear)
| **투구** | **DEF 보정** | **특성** |
|---|:-:|---|
| 가죽 모자 (Leather Cap) | +0 |  |
| 체인 후드 (Chain Coif) | +0 |  |
| 철 투구 (Iron Helm) | +1 | DEX -1 |
| 풀 헬름 (Great Helm) | +1 | DEX -2 |
| 오픈 헬름 (Open-Face Helm) | +0 |  |
| 마법사 모자 (Wizard Hat) | +0 | INT +1 |
| 수정 관 (Crystal Crown) | +0 | MDEF +2 |
### 4.2 갑옷 (Armor)
| **갑옷** | **DEF 보정** | **유형** | **특성** |
|---|:-:|:-:|---|
| 패디드 아머 (Padded) | +1 | 경갑 | 회피 +5% |
| 레더 아머 (Leather) | +2 | 경갑 |  |
| 스터디드 레더 (Studded Leather) | +3 | 경갑 | 회피 -5% |
| 하이드 아머 (Hide) | +4 | 중갑 | DEX -2, 회피 -2% |
| 체인 셔츠 (Chain Shirt) | +5 | 중갑 | DEX -2, 회피 -3% |
| 스케일 메일 (Scale Mail) | +6 | 중갑 | DEX -3, 회피 -4% |
| 브레스트플레이트 (Breastplate) | +7 | 중갑 | DEX -3, 회피 -5% |
| 하프플레이트 (Half Plate) | +8 | 중갑 | DEX -3, 회피 -7% |
| 링 메일 (Ring Mail) | +10 | 중장갑 | DEX -5, 회피 -10% |
| 체인 메일 (Chain Mail) | +11 | 중장갑 | DEX -5, 회피 -5%, MOV -1 |
| 스플린트 아머 (Splint) | +15 | 중장갑 | DEX -7, 회피 -10%, MOV -1 |
| 플레이트 아머 (Plate) | +18 | 중장갑 | DEX -7, 회피 -15%, MOV -2 |
| 로브 (Robe) | +1 | 로브 | INT +1, MDEF +2 |
| 마법 로브 (Magic Robe) | +2 | 로브 | INT +2, MDEF +4 |
### 4.3 방어구 특성 및 유형별 영향
* **경갑(Light)**: 이동과 회피에 미미한 영향, 물리 방어력 낮음
* **중갑(Medium)**: 이동과 회피에 일부 패널티, 물리 방어력 중간
* **중장갑(Heavy)**: 이동과 회피에 큰 패널티, 물리 방어력 높음
* **로브(Robe)**: 마법 방어력과 지능 증가, 물리 방어력 매우 낮음

⠀5. 액세서리 시스템
### 5.1 반지 (Ring)
반지는 양손에 각각 하나씩, 총 2개까지 착용 가능합니다.
| **반지** | **등급** | **효과** |
|---|:-:|---|
| 힘의 반지 (Ring of Strength) | 고급 | STR +2 |
| 민첩의 반지 (Ring of Dexterity) | 고급 | DEX +2 |
| 지능의 반지 (Ring of Intelligence) | 고급 | INT +2 |
| 방어의 반지 (Ring of Defense) | 고급 | DEF +2 |
| 마법 방어의 반지 (Ring of Magic Defense) | 고급 | MDEF +2 |
| 속도의 반지 (Ring of Speed) | 고급 | SPD +2 |
| 행운의 반지 (Ring of Luck) | 고급 | LUK +2 |
| 회피의 반지 (Ring of Evasion) | 희귀 | 회피율 +10% |
| 중독 저항 반지 (Ring of Poison Resistance) | 희귀 | 중독 저항 +50% |
| 불꽃 반지 (Ring of Fire) | 희귀 | 화염 속성 공격 +20% |
| 얼음 반지 (Ring of Ice) | 희귀 | 냉기 속성 공격 +20% |
| 번개 반지 (Ring of Lightning) | 희귀 | 번개 속성 공격 +20% |
| 생명력 반지 (Ring of Vitality) | 영웅 | 최대 HP +10% |
| 치유 반지 (Ring of Healing) | 영웅 | 턴 시작 시 HP 5% 회복 |
| 연속 공격 반지 (Ring of Double Strike) | 영웅 | 연속 공격 확률 +15% |
| 치명타 반지 (Ring of Critical) | 영웅 | 치명타 확률 +10% |
| 시공간 반지 (Ring of Space-Time) | 전설 | 턴 당 이동력 +1, 20% 확률로 행동 후 추가 행동 |
### 5.2 목걸이 (Necklace)
목걸이는 1개만 착용 가능합니다.
| **목걸이** | **등급** | **효과** |
|---|:-:|---|
| 수호자의 목걸이 (Guardian Necklace) | 고급 | DEF +3, MDEF +3 |
| 생명의 목걸이 (Life Necklace) | 고급 | 최대 HP +10% |
| 마력 증폭 목걸이 (Magic Amplification Necklace) | 고급 | 마법 공격력 +10% |
| 저주 보호 목걸이 (Curse Protection Necklace) | 희귀 | 상태이상 지속시간 -1턴 |
| 원소 조화 목걸이 (Elemental Harmony Necklace) | 희귀 | 모든 원소 저항 +10% |
| 생명력 회복 목걸이 (Vitality Recovery Necklace) | 희귀 | 턴 시작 시 HP 5% 회복 |
| 전투 지휘관의 목걸이 (Battle Commander Necklace) | 영웅 | 주변 2칸 내 아군 공격력 +5% |
| 회피 기동 목걸이 (Evasive Maneuver Necklace) | 영웅 | 피격 시 20% 확률로 회피 |
| 원소 지배자의 목걸이 (Elemental Master Necklace) | 전설 | 모든 원소 피해 +15%, 원소 저항 +15% |
## 6. 소모성 아이템 시스템
### 6.1 상태이상 아이템
| **상태이상** | **소모성 아이템 명** | **효과 지속 시간** | **효과 범위** | **효과 설명** |
|---|---|:-:|:-:|---|
| 기절 (Stun) | 기절 수류탄 (Stun Grenade) | 1 턴 | 2 | 대상 기절 상태 부여, 행동 불가 |
| 기절 (Stun) | 강력 기절 수류탄 (Strong Stun Grenade) | 3 턴 | 3 | 대상 기절 상태 부여, 행동 불가 |
| 침묵 (Silence) | 침묵의 두루마리 (Silence Scroll) | 3 턴 | 3 | 대상 마법 사용 불가 |
| 침묵 (Silence) | 정적의 연막탄 (Smoke of Silence) | 2 턴 | 4 | 범위 내 모든 유닛 마법 사용 불가 |
| 장님 (Blindness) | 실명 분말 (Blinding Powder) | 2 턴 | 1 | 대상 이동 불가, 회피율 -50% |
| 장님 (Blindness) | 암흑 폭탄 (Darkness Bomb) | 3 턴 | 2 | 범위 내 모든 유닛 이동 불가, 회피율 -50% |
| 동결 (Freeze) | 냉기 폭탄 (Frost Bomb) | 1 턴 | 2 | 대상 행동 불가, 받는 물리 데미지 25% 증가 |
| 동결 (Freeze) | 한파의 결정체 (Crystal of Frost) | 2 턴 | 3 | 범위 내 모든 유닛 행동 불가, 받는 물리 데미지 25% 증가 |
| 중독 (Poison) | 독병 (Poison Vial) | 5 턴 | 1 | 턴마다 최대 HP의 5% 피해 |
| 중독 (Poison) | 독안개 폭탄 (Poison Cloud Bomb) | 3 턴 | 3 | 범위 내 모든 유닛 턴마다 최대 HP의 5% 피해 |
| 죽음의 독 (Death Venom) | 맹독병 (Death Venom Flask) | 3 턴 | 1 | 턴마다 최대 HP의 30% 피해 |
| 출혈 (Bleeding) | 출혈 표창 (Bleeding Kunai) | 4 턴 | 1 | 턴마다 현재 HP의 10% 피해 |
| 출혈 (Bleeding) | 파쇄 수류탄 (Shrapnel Grenade) | 3 턴 | 2 | 범위 내 모든 유닛 턴마다 현재 HP의 10% 피해 |
| 화상 (Burn) | 화염병 (Fire Flask) | 4 턴 | 2 | 턴마다 최대 HP의 8% 피해, MDEF -2 |
| 화상 (Burn) | 폭발성 화염 포션 (Explosive Fire Potion) | 3 턴 | 3 | 범위 내 모든 유닛 턴마다 최대 HP의 8% 피해, MDEF -2 |
| 감전 (Electrocution) | 감전 구슬 (Shock Orb) | 3 턴 | 1 | 회피 -30%, 방어 -30%, 이동 -2, 공격 명중 -50% |
| 감전 (Electrocution) | 번개 폭탄 (Lightning Bomb) | 2 턴 | 3 | 범위 내 모든 유닛 회피 -30%, 방어 -30%, 이동 -2, 공격 명중 -50% |
| 복합 (Mixed) | 혼돈의 구슬 (Chaos Orb) | 2 턴 | 3 | 범위 내 무작위 상태이상 부여(기절, 침묵, 중독 중 하나) |
| 혼란 (Confusion) | 현혹의 가루 (Powder of Confusion) | 2 턴 | 2 | 범위 내 모든 적 유닛 동료를 랜덤 공격 |
| 약화 (Weakness) | 약화의 물약 (Potion of Weakness) | 3 턴 | 1 | 대상의 STR, DEF, MDEF -30% |
### 6.2 치유형 아이템
| **아이템 명** | **효과** | **사용 범위** |
|---|---|:-:|
| 상처 치료제 (Minor Healing Potion) | HP 10포인트 회복 | 1 |
| 중급 치료제 (Medium Healing Potion) | HP 30포인트 회복 | 1 |
| 천사의 기적 (Angel's Miracle) | HP 완전 회복 | 1 |
| 마나 회복제 (Mana Potion) | MP 10포인트 회복 | 1 |
| 대형 마나 회복제 (Greater Mana Potion) | MP 30포인트 회복 | 1 |
| 마법 정수 (Magic Essence) | MP 완전 회복 | 1 |
| 만병통치약 (Panacea) | 모든 상태이상 치료 | 1 |
| 상태 이상 치료제 (Status Cure) | 중독, 기절, 침묵, 장님, 출혈 치료 | 1 |
| 회복의 빛 (Light of Recovery) | 범위 내 아군 HP 20포인트 회복 | 3 |
| 정화의 물결 (Cleansing Wave) | 범위 내 아군 상태이상 치료 | 3 |
### 6.3 버프/강화 아이템
| **아이템 명** | **효과** | **지속 시간** | **사용 범위** |
|---|---|:-:|:-:|
| 힘의 물약 (Potion of Strength) | STR +5 | 3턴 | 1 |
| 민첩성의 물약 (Potion of Dexterity) | DEX +5 | 3턴 | 1 |
| 지능의 물약 (Potion of Intelligence) | INT +5 | 3턴 | 1 |
| 수호의 물약 (Potion of Protection) | DEF +5, MDEF +5 | 3턴 | 1 |
| 속도의 물약 (Potion of Speed) | SPD +3, MOV +1 | 3턴 | 1 |
| 명중의 물약 (Potion of Accuracy) | 명중률 +20% | 3턴 | 1 |
| 회피의 물약 (Potion of Evasion) | 회피율 +20% | 3턴 | 1 |
| 전투의 북 (Battle Drum) | 범위 내 아군 ATK +10% | 3턴 | 3 |
| 보호의 깃발 (Protective Banner) | 범위 내 아군 DEF, MDEF +10% | 3턴 | 3 |
| 영웅의 포효 (Hero's Roar) | 범위 내 아군 모든 능력치 +2 | 2턴 | 4 |
## 7. 아이템 데이터 구조
### 7.1 통합 아이템 데이터 구조 (JSON)

json
{
  "id": "item_001",
  "name": "철검",
  "engName": "Iron Sword",
  "iconIndex": 12,
  "category": "equipment",
  "type": "weapon",
  "subType": "one_handed",
  "tier": "common",
  "description": "기본적인 한손검. 균형 잡힌 성능을 가지고 있다.",
  "price": 500,
  "maxStack": 1,
  
  "equipment": {
    "slot": "weapon",
    "damageType": "slash",
    "damageValue": "1d8",
    "range": 1,
    "weight": 5,
    "hit": 80,
    "crit": 0,
    "effects": {
      "statMods": [],
      "statusEffects": []
    },
    "requirements": {
      "level": 1,
      "str": 0,
      "jobs": ["Fighter", "Mercenary", "Hero", "Knight"]
    }
  },
  
  "consumable": null,
  "quest": null
}
### 7.2 방어구 예시 (JSON)

json
{
  "id": "item_101",
  "name": "가죽 갑옷",
  "engName": "Leather Armor",
  "iconIndex": 45,
  "category": "equipment",
  "type": "armor",
  "subType": "light",
  "tier": "common",
  "description": "가벼운 가죽으로 만든 갑옷. 이동에 제약이 거의 없다.",
  "price": 400,
  "maxStack": 1,
  
  "equipment": {
    "slot": "armor",
    "def": 3,
    "mdef": 1,
    "weight": 3,
    "effects": {
      "statMods": [],
      "resistances": [],
      "penalties": {}
    },
    "requirements": {
      "level": 1,
      "jobs": ["Fighter", "Mercenary", "Archer", "Thief"]
    }
  },
  
  "consumable": null,
  "quest": null
}
### 7.3 소모성 아이템 예시 (JSON)

json
{
  "id": "item_201",
  "name": "상처 치료제",
  "engName": "Minor Healing Potion",
  "iconIndex": 78,
  "category": "consumable",
  "type": "healing",
  "subType": "hp",
  "tier": "common",
  "description": "작은 상처를 치료하는 포션. HP 10을 회복한다.",
  "price": 100,
  "maxStack": 10,
  
  "equipment": null,
  
  "consumable": {
    "usable": {
      "battlefield": true,
      "worldMap": true,
      "target": "ally",
      "range": 1,
      "aoe": 0
    },
    "effects": {
      "heal": {
        "type": "flat",
        "value": 10
      },
      "statusCure": []
    },
    "actionCost": 1,
    "charges": 1
  },
  
  "quest": null
}
### 7.4 상태이상 아이템 예시 (JSON)

json
{
  "id": "item_301",
  "name": "독병",
  "engName": "Poison Vial",
  "iconIndex": 92,
  "category": "consumable",
  "type": "debuff",
  "subType": "poison",
  "tier": "uncommon",
  "description": "적에게 던지면 중독 상태를 유발한다. 5턴 동안 매 턴 최대 HP의 5%를 잃는다.",
  "price": 200,
  "maxStack": 10,
  
  "equipment": null,
  
  "consumable": {
    "usable": {
      "battlefield": true,
      "worldMap": false,
      "target": "enemy",
      "range": 2,
      "aoe": 1
    },
    "effects": {
      "statusEffect": {
        "type": "poison",
        "chance": 100,
        "duration": 5,
        "value": {
          "type": "percent_max_hp",
          "amount": 5
        }
      }
    },
    "actionCost": 1,
    "charges": 1
  },
  
  "quest": null
}
### 7.5 퀘스트 아이템 예시 (JSON)

json
{
  "id": "item_401",
  "name": "고대 유물",
  "engName": "Ancient Relic",
  "iconIndex": 120,
  "category": "quest",
  "type": "key",
  "subType": "artifact",
  "tier": "unique",
  "description": "고대 문명의 비밀을 간직한 유물. 특정 지역을 열 수 있는 열쇠 역할을 한다.",
  "price": 0,
  "maxStack": 1,
  
  "equipment": null,
  "consumable": null,
  
  "quest": {
    "questId": "quest_005",
    "useAction": "unlockArea",
    "targetAreaId": "dungeon_003",
    "consumeOnUse": false
  }
}
## 8. 액션 소모 규칙
### 8.1 액션 소모 행위
각 유닛은 턴마다 하나의 액션을 수행할 수 있습니다:
* **이동**: 이동력 범위 내 이동
* **공격**: 무기 사용 공격
* **스킬 사용**: 액티브 스킬 사용
* **아이템 사용**: 소모성 아이템 사용

⠀8.2 액션 소모 없는 행위
다음 행위는 액션을 소모하지 않습니다:
* **대기**: 아무 행동 없이 턴 종료
* **상태 확인**: 자신 또는 다른 유닛의 상태 확인
* **지형 확인**: 지형 정보 확인
* **장비 교체**: 인벤토리 내 장비 교체 (전략 모드에서만 가능)

⠀8.3 아이템 사용과 액션
* **전투 중 사용**: 아이템 사용은 1 액션 소모
* **전투 외 사용**: 전략 모드에서 아이템 사용은 액션 소모 없음
* **스택 제한**: 소모성 아이템은 종류별로 최대 10개까지 스택 가능

⠀9. 질문 답변 사항
### 9.1 아이템 강화/업그레이드 시스템
* 현재 버전에서는 아이템 강화/업그레이드 시스템 구현 없음
* 아이템은 획득한 상태 그대로 사용

⠀9.2 직업별 아이템 착용 제한
* 직업별 장비 제한이 있음 (자세한 내용은 전투 룰 디자인 문서 v1.3의 12장 참조)
* 요구사항에 맞지 않는 장비 착용 시 효과 미적용 또는 착용 불가

⠀9.3 소모성 아이템 최대 스택
* 기본적으로 모든 소모성 아이템은 최대 10개까지 스택 가능
* 밸런스에 따라 조정 가능성 있음

⠀9.4 아이템 드롭 및 획득
* 적 유닛 및 상자에서 티어별 아이템 드롭
* 티어별 드롭 확률 및 리스트는 밸런스 디자인 문서에서 추후 다룰 예정
* 상점 시스템은 전략 탐험 모드에서 별도 설계 예정

⠀9.5 소모성 아이템 사용 제한
* 턴당 하나의 액션만 사용 가능 (이동, 공격, 스킬, 아이템 중 택일)
* 액션 소모 없는 행위: 대기, 상태 확인, 지형 확인 등

⠀9.6 특수 아이템
* 퀘스트 아이템 및 키 아이템은 전략 탐험 모드에서 주로 사용
* 전투 모드에서는 일반적으로 사용 불가

⠀9.7 아이템 거래 및 경제
* 전략 탐험 모드에서 상점 시스템 구현 예정
* 추후 설계 예정

⠀9.8 아이템 랜덤 변동
* 동일 명칭 아이템의 랜덤 변동 요소 없음
* 모든 같은 이름 아이템은 동일한 속성 보유

⠀9.9 UI/UX 및 이벤트 아이템
* 추후 별도 논의 예정
