-- ============================================================
--  goodi moodi — Supabase Schema
--  Run this entire file in the Supabase SQL Editor
-- ============================================================

-- ── PRODUCTS ────────────────────────────────────────────────
CREATE TABLE IF NOT EXISTS products (
  id          BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  name        TEXT        NOT NULL,
  emoji       TEXT        NOT NULL DEFAULT '☕',
  category    TEXT        NOT NULL DEFAULT 'Other',
  price       NUMERIC(10,2) NOT NULL DEFAULT 0,
  stock       INTEGER     NOT NULL DEFAULT 0,
  max_stock   INTEGER     NOT NULL DEFAULT 20,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- ── SALES ───────────────────────────────────────────────────
CREATE TABLE IF NOT EXISTS sales (
  id            BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  method        TEXT          NOT NULL DEFAULT 'Cash',
  subtotal      NUMERIC(10,2) NOT NULL DEFAULT 0,
  discount_amt  NUMERIC(10,2) NOT NULL DEFAULT 0,
  total         NUMERIC(10,2) NOT NULL DEFAULT 0,
  given         NUMERIC(10,2) NOT NULL DEFAULT 0,
  note          TEXT,
  voided        BOOLEAN       NOT NULL DEFAULT FALSE,
  created_at    TIMESTAMPTZ   NOT NULL DEFAULT NOW()
);

-- ── SALE ITEMS ──────────────────────────────────────────────
CREATE TABLE IF NOT EXISTS sale_items (
  id            BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  sale_id       BIGINT        NOT NULL REFERENCES sales(id) ON DELETE CASCADE,
  product_name  TEXT          NOT NULL,
  emoji         TEXT,
  qty           INTEGER       NOT NULL DEFAULT 1,
  price         NUMERIC(10,2) NOT NULL DEFAULT 0,
  total         NUMERIC(10,2) NOT NULL DEFAULT 0
);

-- ── STOCK ADJUSTMENTS ───────────────────────────────────────
CREATE TABLE IF NOT EXISTS stock_adjustments (
  id            BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  product_id    BIGINT,
  product_name  TEXT          NOT NULL,
  emoji         TEXT,
  qty_removed   INTEGER       NOT NULL,
  reason        TEXT          NOT NULL DEFAULT 'other',
  note          TEXT,
  created_at    TIMESTAMPTZ   NOT NULL DEFAULT NOW()
);

-- ── ROW LEVEL SECURITY ──────────────────────────────────────
ALTER TABLE products          ENABLE ROW LEVEL SECURITY;
ALTER TABLE sales             ENABLE ROW LEVEL SECURITY;
ALTER TABLE sale_items        ENABLE ROW LEVEL SECURITY;
ALTER TABLE stock_adjustments ENABLE ROW LEVEL SECURITY;

-- Allow full access via anon key (add auth later as needed)
DO $$ BEGIN
  CREATE POLICY "anon_all" ON products          FOR ALL USING (true) WITH CHECK (true);
  CREATE POLICY "anon_all" ON sales             FOR ALL USING (true) WITH CHECK (true);
  CREATE POLICY "anon_all" ON sale_items        FOR ALL USING (true) WITH CHECK (true);
  CREATE POLICY "anon_all" ON stock_adjustments FOR ALL USING (true) WITH CHECK (true);
EXCEPTION WHEN duplicate_object THEN NULL; END $$;

-- ── INDEXES ─────────────────────────────────────────────────
CREATE INDEX IF NOT EXISTS idx_sales_created    ON sales(created_at DESC);
CREATE INDEX IF NOT EXISTS idx_items_sale       ON sale_items(sale_id);
CREATE INDEX IF NOT EXISTS idx_adj_created      ON stock_adjustments(created_at DESC);

-- ── SAMPLE PRODUCTS ─────────────────────────────────────────
INSERT INTO products (name, emoji, category, price, stock, max_stock) VALUES
  ('Espresso',        '☕', 'Coffee', 55,  50, 50),
  ('Latte',           '🥛', 'Coffee', 75,  40, 50),
  ('Americano',       '☕', 'Coffee', 60,  35, 50),
  ('Cappuccino',      '☕', 'Coffee', 80,   3, 50),
  ('Cold Brew',       '🧊', 'Coffee', 95,  12, 30),
  ('Green Tea',       '🍵', 'Tea',    55,  25, 40),
  ('Oolong Tea',      '🫖', 'Tea',    65,   0, 30),
  ('Chamomile',       '🌼', 'Tea',    70,  18, 30),
  ('Croissant',       '🥐', 'Food',   65,   8, 20),
  ('Muffin',          '🧁', 'Food',   60,   2, 20),
  ('Sandwich',        '🥪', 'Food',  120,   6, 15),
  ('Cheesecake',      '🍰', 'Food',  130,   0, 10),
  ('Orange Juice',    '🍊', 'Drinks', 70,  15, 20),
  ('Smoothie',        '🥤', 'Drinks',110,  10, 20),
  ('Water',           '💧', 'Drinks', 20,  50, 50),
  ('Sparkling Water', '🫧', 'Drinks', 35,  20, 30)
ON CONFLICT DO NOTHING;
