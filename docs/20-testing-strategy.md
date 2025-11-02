# 20. Testing Strategy

## 20.1 Unit Tests  
Covers core functionality: render, save, workflow transitions, and rules evaluation.

### 20.1.1 Test Case 1: Create PHO Instance

```js
describe("PHO Creation Tests", () => {
  test("Should create PHO with valid data", async () => {
    const phoData = {
      caller_name: "Mario Rossi",
      phone_number: "+393921234567",
      duration: 240,
      outcome: "INTERESTED",
      notes: "Requests 500 blonde strands"
    };

    const result = await createPHO(phoData);

    expect(result.dna_code).toMatch(/^PHO\\d{6,}$/);
    expect(result.status).toBe("NEW");
    expect(result.k_coefficient).toBeLessThan(1.5);
  });

  test("Should validate required fields", async () => {
    const invalidData = {
      caller_name: "A", // Too short
      phone_number: "invalid"
    };

    await expect(createPHO(invalidData)).rejects.toThrow("Validation failed");
  });
});
```

---

### 20.1.2 Test Case 2: Workflow Transitions

```js
test("PHO Workflow Transitions", async () => {
  const pho = await createPHO(validData);

  // Test valid transition
  await transitionWorkflow(pho.dna_code, "IN_PROGRESS");
  const updated = await getPHO(pho.dna_code);
  expect(updated.status).toBe("IN_PROGRESS");

  // Test invalid transition
  await expect(
    transitionWorkflow(pho.dna_code, "ARCHIVED")
  ).rejects.toThrow("Invalid transition");
});
```

---

## 20.2 Template Tests  
Each seed template is assigned fixtures:  
- Sample `template_json`  
- Sample `instance_json`  
- Consistency tests for domains, validations, and workflows

---

## 20.3 Integration Tests  
Ensure write-through into CRM, external APIs, and first-class entities.

```js
describe("CRM Sync Tests", () => {
  test("Should sync to Salesforce", async () => {
    const pho = await createPHO(testData);
    await wait(1000); // Wait for async sync

    const sfContact = await salesforce.query(
      `SELECT Name, Phone FROM Contact WHERE ExternalId__c = '${pho.dna_code}'`
    );

    expect(sfContact.Name).toBe(testData.caller_name);
    expect(sfContact.Phone).toBe(testData.phone_number);
  });
});
```

---

## 20.4 UAT / Performance Benchmarks  
Run a real process end-to-end with the team and measure time and clarity.

```json
{
  "performance_requirements": {
    "create_pho": {
      "target_ms": 500,
      "max_ms": 1000,
      "includes": ["validation", "save", "workflow_init", "log"]
    },
    "search_pho": {
      "target_ms": 100,
      "max_ms": 300,
      "result_size": 100
    },
    "generate_rap": {
      "target_ms": 2000,
      "max_ms": 5000,
      "includes": ["query_log", "aggregate", "format_pdf"]
    },
    "calculate_k": {
      "target_ms": 50,
      "max_ms": 100
    }
  }
}
```

---

## 20.5 Bug Tracking / K Coefficient Validation  
One list with severity and repro steps; short fix cycles.

```js
// Test K Calculation
function testKCalculation() {
  const testCases = [
    {
      input: {
        extracycles: 1.0,
        performance: 1.0,
        presenteeism: 1.0
      },
      expected: 1.0, // Perfect efficiency
      description: "Theoretical perfection"
    },
    {
      input: {
        extracycles: 1.5,
        performance: 0.8,
        presenteeism: 0.95
      },
      expected: 1.97, // 1.5 / (0.8 * 0.95)
      description: "Typical scenario"
    },
    {
      input: {
        extracycles: 2.0,
        performance: 0.5,
        presenteeism: 1.0
      },
      expected: 4.0, // 2.0 / (0.5 * 1.0)
      description: "Critical inefficiency"
    }
  ];

  testCases.forEach(tc => {
    const result = calculateK(tc.input);
    assert(Math.abs(result - tc.expected) < 0.01, tc.description);
  });
}
```
