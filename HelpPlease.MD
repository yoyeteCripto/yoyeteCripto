I need help with this code, is a trading bot for trading view, the code was created in pine script. Help me, thank you.


//@version=5
strategy("Ejemplo de Estrategia con Cruce de Medias", overlay=true)

// Definición de variables de entrada
ema1 = input.int(20, "EMA 1", minval=1)
ema2 = input.int(50, "EMA 2", minval=1)

// Cálculo de medias móviles exponenciales
ema_fast = ta.ema(close, ema1)
ema_slow = ta.ema(close, ema2)

// Definición de variables para seguimiento de tendencia
strong_bullish = ema_fast > ema_slow and ema_fast[1] <= ema_slow[1]
weak_bullish = ema_fast > ema_slow and ema_fast[1] > ema_slow[1]
strong_bearish = ema_fast < ema_slow and ema_fast[1] >= ema_slow[1]
weak_bearish = ema_fast < ema_slow and ema_fast[1] < ema_slow[1]
highTrend = ta.change(ema_fast) > 0 and ta.change(ema_slow) > 0

// Definición de señales de entrada
buy_signal = crossover(ema_fast, ema_slow) and (weak_bullish or strong_bullish)
sell_signal = crossunder(ema_fast, ema_slow) and (weak_bearish or strong_bearish)

// Exit conditions
if (longCondition)
    strategy.entry("My Long Entry Id", strategy.long, when=longCondition)

else
    strategy.close("Short", comment= "Trend change")


// Definición de órdenes
// Definición de órdenes
if buy_signal and close > sma
    strategy.entry("Long", strategy.long)

if sell_signal and close < sma
    strategy.entry("Short", strategy.short)


// Definición de stop loss y take profit
stop_loss = input.float(1.0, "Stop Loss (%)", step=0.1, minval=0)
take_profit = input.float(2.0, "Take Profit (%)", step=0.1, minval=0)

// Soporte y resistencia
support = ta.sma(low, 50)
resistance = ta.sma(high, 50)

// Bandas de Bollinger
basis = ta.sma(close, 20)
dev = input.float(2, "Desviación estándar", minval=0.5, maxval=3)
upper_band = basis + dev * ta.stdev(close, 20)
lower_band = basis - dev * ta.stdev(close, 20)

// Niveles de sobrecompra y sobreventa
overbought = input.float(80, "Overbought level", minval=0, maxval=100)
oversold = input.float(20, "Oversold level", minval=0, maxval=100)

// Volumen de compras y ventas
buy_volume = ta.volume(close > open)
sell_volume = ta.volume(close < open)

// Pivot Points
pivot = (high + low + close) / 3
s1 = 2 * pp - high
s2 = pivot - (high - low)
s3 = low - 2 * (high - pivot)
r1 = 2 * pivot - low
r2 = pivot + (high - low)
r3 = high + 2 * (pivot - low)

// Ichimoku Clouds
conversion_period = input.int(9, title="Conversion")
base_period = input.int(26, title="Base")
lead_span_b_period = input.int(52, title="Leading Span B")
displacement = input.int(26, title="Displacement")

// Calculo de los valores de Ichimoku Clouds
conversion_line = ta.sma(hl2, conversion_period)
base_line = ta.sma(hl2, base_period)
lead_span_a = ta.avg(conversion_line, base_line)
lead_span_b = ta.sma(hl2, lead_span_b_period)

plot(lead_span_a, color=#8B0000, style=plot.style_linebr, linewidth=1, title="Lead 1")
plot(lead_span_b, color=#008B8B, style=plot.style_linebr, linewidth=1, title="Lead 2")
fill(lead_span_a, lead_span_b, color=ta.crossover(lead_span_a, lead_span_b) ? color.green : color.red, transp=75)

// ATR
atr_period = input.int(14, title="ATR Period")
atr = ta.atr(high, low, close, atr_period)
plot(atr, color=color.blue, linewidth=2, title="ATR")

// ADX
adx_period = input.int(14, title="ADX Period")
dmi = ta.dmi(high, low, close, adx_period)
plot(dmi.adx, color=color.orange, linewidth=2, title="ADX")

// Williams %R
williams_period = input.int(14, title="Williams %R Period")
wr = ta.williamsr(high, low, close, williams_period)
plot(wr, color=color.fuchsia, title="Williams %R")

// Pivot Points
pp = (high + low + close) / 3
r1 = 2 * pp - low
s1 = 2 * pp - high
r2 = pp + (high - low)
s2 = pp - (high - low)
r3 = high + 2 * (pp - low)
s3 = low - 2 * (high - pp)

plot(pp, color=color.purple, title="Pivot Point")
plot(r1, color=color.gray, title="Resistance 1")
plot(s1, color=color.gray, title="Support 1")
plot(r2, color=color.black, title="Resistance 2")
plot(s2, color=color.black, title="Support 2")
plot(r3, color=color.blue, title="Resistance 3")
plot(s3, color=color.blue, title="Support 3")

// Definición de señales de entrada
buy_signal = crossover(ema_fast, ema_slow) and (weak_bullish or strong_bullish) and (close > lead_span_a or close > lead_span_b) and (close > pp)
sell_signal = crossunder(ema_fast, ema_slow) and (weak_bearish or strong_bearish) and (close < lead_span_a or close < lead_span_b) and (close < pp)

// Definición de órdenes
if buy_signal
    strategy.entry("Long", strategy.long)
else if sell_signal
    strategy.entry("Short", strategy.short)


// Definición de stop loss y take profit
stop_loss = input.float(1.0, "Stop Loss (%)", step=0.1, minval=0)
take_profit = input.float(2.0, "Take Profit (%)", step=0.1, minval=0)

// Ichimoku Clouds
conversion_period = input.int(9, title="Conversion")
base_period = input.int(26, title="Base")
lagging_span2_period = input.int(52, title="Lagging Span 2")
displacement = input.int(26, title="Displacement")

donchian(len) => avg(lowest(len), highest(len))
donchian_high = donchian(20)
donchian_low = donchian(20)

// Calculo de indicadores Ichimoku Clouds
conversion = ta.sma(hl2, conversion_period)
base = ta.sma(hl2, base_period)
spana = ta.sma(avg(conversion, base), displacement)
spanb = ta.sma(hl2, lagging_span2_period)

// Definición de señales de entrada
tenkansen_cross = crossover(conversion, base)
kijunsen_cross = crossover(avg(high, low), donchian_high) or crossunder(avg(high, low), donchian_low)
price_above_cloud = close > max(spana, spanb)
price_below_cloud = close < min(spana, spanb)

// Exit conditions
if strategy.position_size > 0 and not (price_above_cloud and tenkansen_cross and kijunsen_cross)
    strategy.close("Long", comment="Cloud break")
if strategy.position_size < 0 and not (price_below_cloud and tenkansen_cross and kijunsen_cross)
    strategy.close("Short", comment="Cloud break")


// Definición de órdenes
if tenkansen_cross and kijunsen_cross and price_above_cloud
    strategy.entry("Long", strategy.long)
if tenkansen_cross and kijunsen_cross and price_below_cloud
    strategy.entry("Short", strategy.short)


// Definición de stop loss y take profit
stop_loss_price = ta.pivothigh(high, 14, 5) * (1 - stop_loss / 100)
take_profit_price = ta.pivotalow(low, 14, 5) * (1 + take_profit / 100)

if strategy.position_size > 0
    strategy.exit("Long", "Long", stop=stop_loss_price, limit=take_profit_price)
if strategy.position_size < 0
    strategy.exit("Short", "Short", stop=stop_loss_price, limit=take_profit_price)


// Definición de colores y estilos de líneas para visualización en gráficos
conversion_color = conversion > base ? color.green : color.red
plot(conversion, color=conversion_color, title="Conversion", linewidth=2)
plot(base, color=color.blue, title="Base", linewidth=2)
fill(spana, spanb, color=color.rgb(96, 96, 96, 80), title="Cloud")

// Code ended
