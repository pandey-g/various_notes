***<============Please start with **Adapter** and follow the rules exactly.==============>***

Perfect 👍 We’re now moving into the **Structural Patterns**.
Let’s begin with the **Adapter Pattern** by following your rules step by step.

---

### 1. **Problem**

Often, in software development, you want to reuse an existing class, but its **interface is incompatible** with what your code expects.

Example:

* Suppose your app works with **JSON data**, but you find a third-party library that only gives you **XML output**.
* You want to use that library without rewriting your entire system.

So the problem: **How to make two incompatible interfaces work together without modifying their source code?**

---

### 2. **Solution**

The **Adapter Pattern** solves this by creating a **middle layer (adapter)** that translates one interface into another.

* The client interacts with the adapter in its familiar way.
* The adapter internally converts those calls into the format the “adaptee” (incompatible class) understands.

Thus, the client doesn’t need to change — it just plugs into the adapter.

---

### 3. **Real-world Intuition / Analogy**

Think of an **electric plug adapter**.

* You have a laptop with a **US plug**, but you’re in Europe with **EU sockets**.
* Instead of changing the laptop or the socket, you just use an **adapter**.
* The adapter converts the plug shape/signals, so they work together.

That’s exactly what happens in software!

---

### 4. **Python Example**

```python
# Adaptee (incompatible class)
class XMLDataProvider:
    def get_xml_data(self):
        return "<data><item>Example</item></data>"

# Target interface (what client expects)
class JSONDataTarget:
    def get_json_data(self):
        raise NotImplementedError

# Adapter: converts XML to JSON
import xmltodict
import json

class XMLtoJSONAdapter(JSONDataTarget):
    def __init__(self, xml_provider: XMLDataProvider):
        self.xml_provider = xml_provider

    def get_json_data(self):
        xml_data = self.xml_provider.get_xml_data()
        dict_data = xmltodict.parse(xml_data)     # convert XML → dict
        return json.dumps(dict_data)             # convert dict → JSON

# Client code
def client_code(data_provider: JSONDataTarget):
    print("Client needs JSON:")
    print(data_provider.get_json_data())

# Usage
xml_provider = XMLDataProvider()
adapter = XMLtoJSONAdapter(xml_provider)
client_code(adapter)
```

**Output:**

```
Client needs JSON:
{"data": {"item": "Example"}}
```

Here:

* Client expects JSON (`JSONDataTarget`).
* Existing class (`XMLDataProvider`) gives XML.
* Adapter (`XMLtoJSONAdapter`) converts XML → JSON.

---

### 5. **Check-in**

Did you understand the **Adapter Pattern** with the electric plug analogy and the Python example?
Would you like me to move on to the **Bridge Pattern** next?
