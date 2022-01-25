# DAX Challenge - Can this be optimised?
This code is a real-world example of a calculated column which draws on several tables, using RELATED, RELATEDTABLE, 1-N and N-1 relationships
I translated it from a previous SQL Query - it's huge, the challenge is... can this be optimised?

I am hoping it highlights the power of DAX calculated columns - I would love to be able to do this in PowerQuery, but due to the complexity of tables and relationships, it would be massively expensive and likely super inefficient! Thoughts and comments welcome :) 

```js
Provision Types =
/* Calculated Column: SWITCH statements to categorise Provision Types based on LARS files - LARS Delivery; LARS Category Ref (Legal Entitlement) and Basic Skills Type (Annual Value). This is a translation of a SQL query. There order of the arguments should not impact the result. Each argument is mutually exclusive */
SWITCH (
    TRUE,
    OccReportBI[Funding model] = 35
        && ( OccReportBI[LDFAM type - LDM (A)] = 378
        || OccReportBI[LDFAM type - LDM (B)] = 378
        || OccReportBI[LDFAM type - LDM (C)] = 378
        || OccReportBI[LDFAM type - LDM (D)] = 378
        || OccReportBI[LDFAM type - LDM (E)] = 378
        || OccReportBI[LDFAM type - LDM (F)] = 378 )
        && OccReportBI[Age] < 24, "L3 Offer | 19-23",
    OccReportBI[Funding model] = 35
        && ( OccReportBI[LDFAM type - LDM (A)] = 378
        || OccReportBI[LDFAM type - LDM (B)] = 378
        || OccReportBI[LDFAM type - LDM (C)] = 378
        || OccReportBI[LDFAM type - LDM (D)] = 378
        || OccReportBI[LDFAM type - LDM (E)] = 378
        || OccReportBI[LDFAM type - LDM (F)] = 378 )
        && OccReportBI[Carryover KEY] = 1
        && OccReportBI[Age] >= 24, "Level 3 Adult Offer",
    LEFT ( OccReportBI[Learning aim reference], 1 ) = "Z"
        && CONTAINS (
            RELATEDTABLE ( LegalEntitlement ),
            LegalEntitlement[Non-regulated], 1
        )
        && OR (
            CONTAINS (
                RELATEDTABLE ( LegalEntitlement ),
                LegalEntitlement[CategoryRef], 23
            ),
            CONTAINS (
                RELATEDTABLE ( LegalEntitlement ),
                LegalEntitlement[CategoryRef], 24
            )
        ), "Non-regulated",
    CONTAINS ( RELATEDTABLE ( LegalEntitlement ), LegalEntitlement[MCA Aims], 1 )
        && LEFT ( OccReportBI[Learning aim reference], 4 ) = "Z000"
        || ( OccReportBI[LDFAM type - DAM (A)] = 40
        || OccReportBI[LDFAM type - DAM (A)] = 40
        || OccReportBI[LDFAM type - DAM (B)] = 40
        || OccReportBI[LDFAM type - DAM (C)] = 40
        || OccReportBI[LDFAM type - DAM (D)] = 40
        || OccReportBI[LDFAM type - DAM (E)] = 40
        || OccReportBI[LDFAM type - DAM (F)] = 40 ), "Bespoke Employer Led Programme",
    /* Work Preparation */
    OccReportBI[Notional NVQ level]
        IN { "X", "E", "1", "2", "3" }
            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) = 14
            && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                IN { "1439", "3" }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 11, 20, 29, 31, 33, 12, 19, 30, 32, 34, 35, 36, 37, 38, 39 }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Work Preparation",
    /* Life Skills Maths */
    LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
        && RELATED ( LARSDelivery[BasicSkillsType] ) IN { 12, 19, 30, 32, 34, 35 }, "Life Skills Maths",
    /* Life Skills English */
    LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
        && RELATED ( LARSDelivery[BasicSkillsType] ) IN { 11, 20, 29, 31, 33 }, "Life Skills English",
    /* Digital Entitlement */
    CONTAINS (
        RELATEDTABLE ( LegalEntitlement ),
        LegalEntitlement[Digital Entitlement], 1
    ), "Life Skills Digital",
    /* ESOL */
    RELATED ( LARSDelivery[BasicSkillsType] )
        IN { 22, 26, 27, 28, 36, 37, 38, 39, 40, 41, 42 }, "ESOL",
    /* Pre-Level 2 Vocational */
    OccReportBI[Notional NVQ level]
        IN { "X", "E", "1" }
            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14
            && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                IN { "3", "1422", "2999" }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 36, 37, 38, 39, 43 }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Pre-Level 2 Vocational",
    RELATED ( LARSDelivery[LearnAimRefType] ) = "1439"
        && OccReportBI[Notional NVQ level]
            IN { "X", "E", "1" }
                && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) = 14
                && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                    IN { "3", "1422", "2999" }
                        && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                            IN { 19, 20, 29, 30, 31, 32, 33 }
                                && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                                    IN { 36, 37, 38, 39, 43 }
                                        && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Pre-Level 2 Vocational",
    OccReportBI[Notional NVQ level]
        IN { "X", "E", "1" }
            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) = 14
            && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                IN { "3", "1422", "2999" }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 19, 20, 29, 30, 31, 32, 33 }
                            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                                IN { 36, 37, 38, 39, 43 }
                                    && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Pre-Level 2 Vocational",
    /* Level 2 Vocational */
    OccReportBI[Notional NVQ level]
        IN { "2" } /* LARSCAT.CategoryRef not IN (1,2) and */
            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                IN { 36, 37, 38, 39 }
                    && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14
                    && CONTAINS (
                        RELATEDTABLE ( LegalEntitlement ),
                        LegalEntitlement[Digital Entitlement], 0
                    ) /* and CategoryRef not in (39) */
                    && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                        IN { "3", "1422", "2999" }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            && OccReportBI[Entitlement category (level 2 or 3)] = "No", "Level 2 Vocational",
    /* CategoryRef is Null and */
    NOT ( RELATED ( LARSType[LearnAimRefType] ) )
        IN { "3", "1422", "2999" }
            && OccReportBI[Notional NVQ level] = "2" // IN { "2" }
            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14
            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                IN { 19, 20, 29, 30, 31, 32, 33 }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 36, 37, 38, 39 }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            && OccReportBI[Entitlement category (level 2 or 3)] = "No", "Level 2 Vocational",
    RELATED ( LARSType[LearnAimRefType] ) = "1439"
        && OccReportBI[Notional NVQ level]
            IN { "2" }
                && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) = 14
                && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                    IN { 36, 37, 38, 39 }
                        && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                            IN { 19, 20, 29, 30, 31, 32, 33 }
                                && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                                && OccReportBI[Entitlement category (level 2 or 3)] = "No", "Level 2 Vocational",
    /* Level 2 Vocational */
    OccReportBI[Notional NVQ level]
        IN { "2" } /* LARSCAT.CategoryRef not IN (1,2) and */
            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                IN { 36, 37, 38, 39 }
                    && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14
                    && CONTAINS (
                        RELATEDTABLE ( LegalEntitlement ),
                        LegalEntitlement[Digital Entitlement], 0
                    ) /* and CategoryRef not in (39) */
                    && NOT ( RELATED ( LARSType[LearnAimRefType] ) )
                        IN { "3", "1422", "2999" }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            && OccReportBI[Entitlement category (level 2 or 3)] = "Yes", "Full Level 2 Vocational",
    /* CategoryRef is Null and */
    NOT ( RELATED ( LARSType[LearnAimRefType] ) )
        IN { "3", "1422", "2999" }
            && OccReportBI[Notional NVQ level] = "2" // IN { "2" }
            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14
            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                IN { 19, 20, 29, 30, 31, 32, 33 }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 36, 37, 38, 39 }
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            && OccReportBI[Entitlement category (level 2 or 3)] = "Yes", "Full Level 2 Vocational",
    RELATED ( LARSType[LearnAimRefType] ) = "1439"
        && OccReportBI[Notional NVQ level]
            IN { "2" }
                && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) = 14
                && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                    IN { 36, 37, 38, 39 }
                        && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                            IN { 19, 20, 29, 30, 31, 32, 33 }
                                && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                                && OccReportBI[Entitlement category (level 2 or 3)] = "Yes", "Full Level 2 Vocational",
    OccReportBI[Notional NVQ level]
        IN { "3" }
            && NOT ( RELATED ( LARSDelivery[LearnAimRefType] ) )
                IN {
                    "2",
                    "3",
                    "1422",
                    "2999",
                    "1",
                    "1413",
                    "1430",
                    "1431",
                    "1432",
                    "1433",
                    "1434",
                    "1435",
                    "1453"
                }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 36, 37, 38, 39 }
                            && RELATED ( LARSDelivery[AccessHEIndicator] ) = 0
                            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14 /* and CategoryRef not IN (39) */
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            && OccReportBI[Entitlement category (level 2 or 3)] = "Yes"
                            || /* CategoryRef is null and */
                            OccReportBI[Notional NVQ level]
                                IN { "3" }
                                    && NOT ( RELATED ( LARSDelivery[LearnAimRefType] ) )
                                        IN {
                                            "2",
                                            "3",
                                            "1422",
                                            "2999",
                                            "1",
                                            "1413",
                                            "1430",
                                            "1431",
                                            "1432",
                                            "1433",
                                            "1434",
                                            "1435",
                                            "1453"
                                        }
                                            && RELATED ( LARSDelivery[AccessHEIndicator] ) = 0
                                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                                            && OccReportBI[Entitlement category (level 2 or 3)] = "Yes", "Full Level 3 Vocational",
    OccReportBI[Notional NVQ level]
        IN { "3" }
            && NOT ( RELATED ( LARSDelivery[LearnAimRefType] ) )
                IN {
                    "2",
                    "3",
                    "1422",
                    "2999",
                    "1",
                    "1413",
                    "1430",
                    "1431",
                    "1432",
                    "1433",
                    "1434",
                    "1435",
                    "1453"
                }
                    && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                        IN { 36, 37, 38, 39 }
                            && RELATED ( LARSDelivery[AccessHEIndicator] ) = 0
                            && RELATED ( LARSDelivery[SectorSubjectAreaTier1] ) <> 14 /* and CategoryRef not IN (39) */
                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z"
                            || /* CategoryRef is null and */
                            OccReportBI[Notional NVQ level]
                                IN { "3" }
                                    && NOT ( RELATED ( LARSDelivery[LearnAimRefType] ) )
                                        IN {
                                            "2",
                                            "3",
                                            "1422",
                                            "2999",
                                            "1",
                                            "1413",
                                            "1430",
                                            "1431",
                                            "1432",
                                            "1433",
                                            "1434",
                                            "1435",
                                            "1453"
                                        }
                                            && RELATED ( LARSDelivery[AccessHEIndicator] ) = 0
                                            && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Level 3 Vocational",
    /* Access to HE */
    RELATED ( LARSDelivery[AccessHEIndicator] ) = 1
        || RELATED ( LARSDelivery[AccessHEIndicator] ) = 1
            && OccReportBI[Delivery Type] = "Distance", "Access to HE",
    /* this will create a count of distance learning quals but it also includes the provision type  */
    RELATED ( LARSDelivery[LearnAimRefType] )
        IN {
            "2",
            "3",
            "1422",
            "2999",
            "1",
            "1413",
            "1430",
            "1431",
            "1432",
            "1433",
            "1434",
            "1435",
            "1453"
        }
            && NOT ( RELATED ( LARSDelivery[BasicSkillsType] ) )
                IN { 11, 12 }
                    && LEFT ( OccReportBI[Learning aim reference], 1 ) <> "Z", "Academic Quals",
    // CONTAINS(RELATEDTABLE( LegalEntitlement), LegalEntitlement[MCA Aims], 1 ),
    // "Skills Innovation Pilot",
    "Other"
)

```
