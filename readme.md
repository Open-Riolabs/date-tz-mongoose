
# 📅 DateTzSchema for Mongoose

A custom Mongoose schema type for handling timezone-aware dates using the [`@open-rlb/date-tz`](https://www.npmjs.com/package/@open-rlb/date-tz) library.

## ✨ Features

- Stores date and time as a structured object with `timestamp` and `timezone`.
- Supports casting plain objects to `DateTz` instances.
- Easily integrates into Mongoose schemas.
- Type-safe with `IDateTz` interface.

---

## 📦 Installation

```bash
npm install @open-rlb/date-tz
```

Add the custom schema type file (e.g. `date-tz.schema.ts`) to your project.

---

## 📚 What is `DateTzSchema`?

This custom schema type lets you store timezone-aware date objects in MongoDB using Mongoose. It wraps around the `DateTz` class from the `@open-rlb/date-tz` package and ensures your application consistently handles date/times across timezones.

The expected shape is:

```ts
{
  timestamp: number;   // Unix timestamp in milliseconds
  timezone: string;    // IANA timezone string (e.g., "Europe/Rome")
}
```

---

## 🧠 Usage

### 1. Register the Custom Type

Ensure `DateTzSchema` is registered once in your application:

```ts
import mongoose from "mongoose";
import { DateTzSchema } from "./date-tz.schema"; // adjust path as needed

mongoose.Schema.Types['DateTzSchema'] = DateTzSchema;
```

### 2. Define Your Schema

```ts
import mongoose from "mongoose";

const BookingSchema = new mongoose.Schema({
  pickupDate: {
    type: mongoose.Schema.Types['DateTzSchema'],
    required: true,
  },
});
```

### 3. Use with TypeScript

```ts
import { DateTz } from "@open-rlb/date-tz";

const booking = new BookingModel({
  pickupDate: new DateTz(Date.now(), "Europe/Rome"),
});

await booking.save();
```

---

## 🔎 Example Document in MongoDB

```json
{
  "pickupDate": {
    "timestamp": 1750506300000,
    "timezone": "Europe/Rome"
  }
}
```

---

## 🧪 Testing Cast Behavior

This schema type will automatically cast objects that match `{ timestamp: number, timezone: string }` into `DateTz` instances.

```ts
const raw = {
  timestamp: 1750506300000,
  timezone: "Europe/Rome"
};

const casted = new DateTzSchema('pickupDate', {}).cast(raw);
console.log(casted instanceof DateTz); // true
```

---

## 🛠️ Development Notes

- The `cast()` method currently supports:
  - `DateTz` instances (returned as-is)
  - Plain objects with `timestamp` and `timezone` keys
- String parsing is commented out — you can enable it if you need to parse formatted strings.

---

## 📄 License

MIT
