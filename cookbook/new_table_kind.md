# A new kind from a table row

**Goal**: add one more kind of something (a weapon, an enemy, an item, a box) that differs from the others only by numbers, without new code.
**From**: the sandbox sample's box kinds (`samples/sandbox`). A weapon or an enemy kind is the same pattern with other fields.

Many things of the same shape are rows of a **definition table**. A row is data, so a designer adds one in the editor's [Tables] panel (or a spreadsheet through CSV) and no build is needed. Rows of simulation tables go into the content hash, so every peer must have the same rows.

## 1. Declare the row type and the table

The row type lists the numbers a kind has, with defaults. `MEALKIT_TABLE` ties it to a file:

% from: samples/sandbox/sim/Tables.h
```cpp
MEALKIT_STRUCT(PropKind) {
    MEALKIT_FIELD(mealkit::f32, sizeMin, 0.28f, "반 크기 최소 (m)");
    MEALKIT_FIELD(mealkit::f32, density, 200.0f, "밀도 (kg/m³)|range=1..2000");
    MEALKIT_FIELD(mealkit::f32, restitution, 0.15f, "반발|range=0..1");
};
MEALKIT_TABLE(PropKind, "tables/props.table", 0); // simulation table: goes into the content hash
```

The last argument is `0` for a table the simulation reads. A table only the view reads (icons, names shown to the player) uses `kTableView` and stays out of the hash. Adding a field later is safe: rows without it get the default.

## 2. Add the row

A row is its id and the values that differ from the defaults:

% from: samples/sandbox/assets/tables/props.table
```text
row bouncy
sizeMin 0.200000003
sizeMax 0.400000006
density 80
restitution 0.800000012
```

This is the whole change for a new kind. The id (`bouncy`) is the name other data uses.

## 3. Point at the row from other data

Scene objects, other tables and loadouts refer to a row with `RowRef<Row>`: the file holds the id, the editor shows a dropdown of the table's rows:

% from: samples/sandbox/sim/Objects.h
```cpp
    MEALKIT_FIELD(mealkit::RowRef<PropKind>, kind, "crate", "상자 종류");
```

Place the object in MkRender and pick `bouncy` in its inspector.

## 4. Read the row by index in the simulation

State never holds names ({doc}`../manual/game_code`). When the match starts, turn the id into a row number once, and warn if the row is missing:

% from: samples/sandbox/sim/World.cpp
```cpp
            u32 kind = world.propKinds.find(def.kind.id); // row name → index (once at load)
            if (kind == mealkit::kNoRow) {
                kind = 0;
            }
```

From then on `sim/` reads the row by number, which is a plain array lookup:

% from: samples/sandbox/sim/Props.cpp
```cpp
        const PropKind& t = world.propKinds[area.kind];
```

For a sound, an effect or a shake when something of the new kind is hit, add a feedback row ({doc}`event_feedback`); it doesn't need the table.
