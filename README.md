[メナード注文管理.html](https://github.com/user-attachments/files/26505277/default.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>メナード注文管理</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Hiragino Sans', 'Yu Gothic', sans-serif; background: #fdf6f9; color: #333; font-size: 14px; }

  /* ヘッダー */
  .header { background: linear-gradient(135deg, #c0607a, #e8a0b0); color: white; padding: 12px 16px; display: flex; align-items: center; gap: 10px; box-shadow: 0 2px 6px rgba(0,0,0,0.2); position: sticky; top: 0; z-index: 100; }
  .header h1 { font-size: 18px; font-weight: bold; }
  .header .logo { font-size: 24px; }

  /* メインレイアウト */
  .container { display: flex; height: calc(100vh - 52px); }

  /* 左側: 商品リスト */
  .product-panel { flex: 1; display: flex; flex-direction: column; overflow: hidden; border-right: 2px solid #e8c0cc; }

  /* カテゴリタブ */
  .category-tabs { display: flex; flex-wrap: wrap; gap: 4px; padding: 10px; background: #fff; border-bottom: 1px solid #e8c0cc; }
  .cat-btn { padding: 5px 10px; border: 1.5px solid #c0607a; border-radius: 20px; background: white; color: #c0607a; cursor: pointer; font-size: 12px; transition: all 0.2s; white-space: nowrap; }
  .cat-btn.active, .cat-btn:hover { background: #c0607a; color: white; }

  /* 検索 */
  .search-box { padding: 10px; background: #fff; border-bottom: 1px solid #e8c0cc; }
  .search-box input { width: 100%; padding: 8px 12px; border: 1.5px solid #ddb0c0; border-radius: 20px; font-size: 14px; outline: none; }
  .search-box input:focus { border-color: #c0607a; }

  /* 商品一覧 */
  .product-list { flex: 1; overflow-y: auto; padding: 8px; }
  .product-item { display: flex; align-items: center; padding: 8px 10px; margin-bottom: 4px; background: white; border-radius: 8px; border: 1px solid #f0d0da; cursor: pointer; transition: all 0.15s; gap: 8px; }
  .product-item:hover { border-color: #c0607a; background: #fff5f8; transform: translateX(2px); }
  .product-item .cat-badge { font-size: 10px; background: #f0d0da; color: #c0607a; padding: 2px 6px; border-radius: 10px; white-space: nowrap; flex-shrink: 0; }
  .product-item .prod-name { flex: 1; font-size: 13px; line-height: 1.4; }
  .product-item .prod-price { font-size: 13px; font-weight: bold; color: #c0607a; white-space: nowrap; }
  .product-item .prod-vol { font-size: 11px; color: #999; }
  .product-item .add-btn { background: #c0607a; color: white; border: none; border-radius: 50%; width: 26px; height: 26px; font-size: 18px; cursor: pointer; line-height: 1; flex-shrink: 0; display: flex; align-items: center; justify-content: center; }
  .product-item .add-btn:hover { background: #a0485e; }

  /* 右側: 注文パネル */
  .order-panel { width: 360px; display: flex; flex-direction: column; background: white; }

  /* 顧客情報 */
  .customer-info { padding: 12px; background: #fff5f8; border-bottom: 2px solid #e8c0cc; }
  .customer-info h2 { font-size: 15px; color: #c0607a; margin-bottom: 8px; }
  .customer-info input, .customer-info select { width: 100%; padding: 7px 10px; border: 1.5px solid #ddb0c0; border-radius: 6px; font-size: 13px; margin-bottom: 6px; outline: none; }
  .customer-info input:focus { border-color: #c0607a; }
  .row2 { display: flex; gap: 6px; }
  .row2 input { margin-bottom: 0; }

  /* 注文リスト */
  .order-list-wrap { flex: 1; overflow-y: auto; padding: 8px; }
  .order-list-wrap h3 { font-size: 13px; color: #888; margin-bottom: 8px; padding-bottom: 4px; border-bottom: 1px solid #eee; }
  .order-empty { text-align: center; color: #ccc; padding: 30px; font-size: 13px; }
  .order-item { display: flex; align-items: center; gap: 6px; padding: 7px 8px; background: #fff5f8; border-radius: 6px; margin-bottom: 4px; border: 1px solid #f0d0da; }
  .order-item .o-name { flex: 1; font-size: 12px; line-height: 1.3; }
  .order-item .qty-ctrl { display: flex; align-items: center; gap: 3px; }
  .qty-btn { background: #e8c0cc; border: none; border-radius: 4px; width: 22px; height: 22px; cursor: pointer; font-size: 14px; display: flex; align-items: center; justify-content: center; }
  .qty-btn:hover { background: #c0607a; color: white; }
  .qty-num { font-size: 13px; font-weight: bold; min-width: 20px; text-align: center; }
  .o-subtotal { font-size: 12px; font-weight: bold; color: #c0607a; min-width: 70px; text-align: right; }
  .del-btn { background: none; border: none; color: #ccc; cursor: pointer; font-size: 16px; padding: 0 2px; }
  .del-btn:hover { color: #e00; }

  /* 合計 */
  .total-section { padding: 12px; border-top: 2px solid #e8c0cc; background: #fff5f8; }
  .total-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; font-size: 13px; }
  .total-row.grand { font-size: 16px; font-weight: bold; color: #c0607a; margin-top: 6px; padding-top: 6px; border-top: 1px solid #e8c0cc; }

  /* ボタン */
  .action-buttons { padding: 10px 12px; display: flex; flex-direction: column; gap: 6px; }
  .btn { padding: 10px; border: none; border-radius: 8px; cursor: pointer; font-size: 14px; font-weight: bold; transition: all 0.2s; }
  .btn-print { background: linear-gradient(135deg, #c0607a, #e8a0b0); color: white; }
  .btn-print:hover { background: linear-gradient(135deg, #a0485e, #c0607a); }
  .btn-save { background: #4a9060; color: white; }
  .btn-save:hover { background: #3a7050; }
  .btn-line { background: linear-gradient(135deg, #06c755, #00a040); color: white; }
  .btn-line:hover { background: linear-gradient(135deg, #00a040, #008030); }
  .btn-clear { background: #f0f0f0; color: #888; }
  .btn-clear:hover { background: #e0e0e0; color: #555; }

  /* 履歴ボタン */
  .history-btn { position: fixed; bottom: 20px; left: 20px; background: #7878c0; color: white; border: none; border-radius: 50%; width: 50px; height: 50px; font-size: 22px; cursor: pointer; box-shadow: 0 3px 10px rgba(0,0,0,0.2); }

  /* モーダル */
  .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 200; align-items: center; justify-content: center; }
  .modal.show { display: flex; }
  .modal-box { background: white; border-radius: 12px; padding: 20px; width: 90%; max-width: 600px; max-height: 80vh; display: flex; flex-direction: column; }
  .modal-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
  .modal-header h2 { font-size: 16px; color: #c0607a; }
  .modal-close { background: none; border: none; font-size: 24px; cursor: pointer; color: #888; }
  .modal-body { flex: 1; overflow-y: auto; }
  .history-entry { background: #fff5f8; border-radius: 8px; padding: 12px; margin-bottom: 10px; border: 1px solid #f0d0da; cursor: pointer; }
  .history-entry:hover { border-color: #c0607a; }
  .history-entry .h-meta { font-size: 12px; color: #888; margin-bottom: 4px; }
  .history-entry .h-customer { font-size: 14px; font-weight: bold; color: #333; }
  .history-entry .h-total { font-size: 13px; color: #c0607a; font-weight: bold; }
  .history-entry .h-items { font-size: 11px; color: #666; margin-top: 4px; }
  .no-history { text-align: center; color: #ccc; padding: 30px; }

  /* 印刷用 */
  @media print {
    body { background: white; font-size: 12px; }
    .header, .product-panel, .action-buttons, .history-btn, .modal { display: none !important; }
    .container { display: block; height: auto; }
    .order-panel { width: 100%; border: none; }
    .customer-info input, .customer-info select { border: none; font-weight: bold; }
    .order-item { break-inside: avoid; }
    .print-title { display: block !important; font-size: 20px; text-align: center; color: #c0607a; font-weight: bold; margin-bottom: 16px; }
    .btn-print, .btn-save, .btn-clear { display: none; }
  }
  .print-title { display: none; }

  /* レスポンシブ */
  @media (max-width: 700px) {
    .container { flex-direction: column; height: auto; }
    .order-panel { width: 100%; border-top: 2px solid #e8c0cc; }
    .product-list { max-height: 50vh; }
    .order-list-wrap { max-height: 40vh; }
  }
</style>
</head>
<body>

<div class="header">
  <span class="logo">🌸</span>
  <h1>メナード 注文管理</h1>
</div>

<div class="container">
  <!-- 左: 商品パネル -->
  <div class="product-panel">
    <div class="category-tabs" id="catTabs"></div>
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="🔍 商品名で検索...">
    </div>
    <div class="product-list" id="productList"></div>
  </div>

  <!-- 右: 注文パネル -->
  <div class="order-panel">
    <div class="customer-info">
      <h2>📋 注文情報</h2>
      <input type="text" id="customerName" placeholder="お客様名" />
      <div class="row2">
        <input type="date" id="orderDate" />
        <input type="text" id="orderNote" placeholder="メモ" />
      </div>
    </div>

    <div class="order-list-wrap">
      <h3>注文商品</h3>
      <div id="orderItems"><div class="order-empty">← 左の商品を選んでください</div></div>
    </div>

    <div class="total-section">
      <div class="total-row"><span>小計（税抜）</span><span id="subtotalEx">0円</span></div>
      <div class="total-row"><span>小計（税込）</span><span id="subtotalTax">0円</span></div>
      <div class="total-row grand"><span>合計（税込）</span><span id="grandTotal">0円</span></div>
    </div>

    <div class="action-buttons">
      <div class="print-title">メナード注文書</div>
      <button class="btn btn-print" onclick="printOrder()">🖨️ 印刷する</button>
      <button class="btn btn-line" onclick="sendToLINE()">💬 LINEで注文を送る</button>
      <button class="btn btn-save" onclick="saveOrder()">💾 注文を保存</button>
      <button class="btn btn-clear" onclick="clearOrder()">🗑️ クリア</button>
    </div>
  </div>
</div>

<button class="history-btn" onclick="showHistory()" title="注文履歴">📋</button>

<!-- 履歴モーダル -->
<div class="modal" id="historyModal">
  <div class="modal-box">
    <div class="modal-header">
      <h2>📋 注文履歴</h2>
      <button class="modal-close" onclick="closeHistory()">×</button>
    </div>
    <div class="modal-body" id="historyBody"></div>
  </div>
</div>

<script>
const PRODUCTS = [{"cat": "スキンケア", "name": "エンベリエ リフレッシュマッサージクレンジング・マッサージクリーム", "priceEx": "20,000円", "priceTax": "22,000円", "vol": "160g"}, {"cat": "スキンケア", "name": "イルネージュ リフレッシュマッサージクレンジング・マッサージクリーム", "priceEx": "13,000円", "priceTax": "14,300円", "vol": "150g"}, {"cat": "スキンケア", "name": "イルネージュ リフレッシュマッサージ（無香料）クレンジング・マッサージクリーム", "priceEx": "13,000円", "priceTax": "14,300円", "vol": "150g"}, {"cat": "スキンケア", "name": "リシアル クレンジングクリーム", "priceEx": "5,500円", "priceTax": "6,050円", "vol": "130g"}, {"cat": "スキンケア", "name": "TK クレンジングクリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "130g"}, {"cat": "スキンケア", "name": "TK クレンジングクリーム（無香料）クレンジングクリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "130g"}, {"cat": "スキンケア", "name": "メリーゼ クレンジングクリーム", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "130g"}, {"cat": "スキンケア", "name": "デルピア Ⅱ クレンジングクリーム", "priceEx": "3,200円", "priceTax": "3,520円", "vol": "110g"}, {"cat": "スキンケア", "name": "クイックレンジングシートクレンジングシート", "priceEx": "300円", "priceTax": "330円", "vol": "10枚"}, {"cat": "スキンケア", "name": "エンベリエ ウオッシング洗顔クリーム", "priceEx": "12,000円", "priceTax": "13,200円", "vol": "130g"}, {"cat": "スキンケア", "name": "イルネージュ ウオッシング洗顔クリーム", "priceEx": "9,000円", "priceTax": "9,900円", "vol": "130g"}, {"cat": "スキンケア", "name": "イルネージュ ウオッシング（無香料）洗顔クリーム", "priceEx": "9,000円", "priceTax": "9,900円", "vol": "130g"}, {"cat": "スキンケア", "name": "リシアル ウオッシングクリーム", "priceEx": "5,500円", "priceTax": "6,050円", "vol": "130g"}, {"cat": "スキンケア", "name": "TK ウオッシングクリーム洗顔クリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "130g"}, {"cat": "スキンケア", "name": "TK ウオッシングクリーム（無香料）洗顔クリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "130g"}, {"cat": "スキンケア", "name": "メリーゼ ウオッシングクリーム洗顔クリーム", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "130g"}, {"cat": "スキンケア", "name": "デルピア Ⅱ ウオッシングクリーム洗顔クリーム", "priceEx": "3,200円", "priceTax": "3,520円", "vol": "130g"}, {"cat": "スキンケア", "name": "ロマネス アクネ ウオッシングクリーム洗顔クリーム", "priceEx": "1,740円", "priceTax": "1,914円", "vol": "120g"}, {"cat": "スキンケア", "name": "フェアルーセント 活肌緑精洗顔石けん", "priceEx": "3,800円", "priceTax": "4,180円", "vol": "標準重量135g"}, {"cat": "スキンケア", "name": "薬用ビューネ ソープ洗顔石けん", "priceEx": "2,800円", "priceTax": "3,080円", "vol": "120g"}, {"cat": "スキンケア", "name": "リシアル マッサージクリーム", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "80g"}, {"cat": "スキンケア", "name": "TK マッサージクリーム", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "80g"}, {"cat": "スキンケア", "name": "TK マッサージクリーム（無香料）マッサージクリーム", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "80g"}, {"cat": "スキンケア", "name": "デルピア Ⅱ マッサージクリーム", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "80g"}, {"cat": "スキンケア", "name": "オーセント リクイド （容器付） 化粧液", "priceEx": "70,000円", "priceTax": "77,000円", "vol": "70mL"}, {"cat": "スキンケア", "name": "エンベリエ リクイド化粧液", "priceEx": "30,000円", "priceTax": "33,000円", "vol": "130mL"}, {"cat": "スキンケア", "name": "イルネージュ ローション化粧水・ローション", "priceEx": "12,000円", "priceTax": "13,200円", "vol": "130mL"}, {"cat": "スキンケア", "name": "イルネージュ ローション（無香料）化粧水・ローション", "priceEx": "12,000円", "priceTax": "13,200円", "vol": "130mL"}, {"cat": "スキンケア", "name": "リシアル ローション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "150mL"}, {"cat": "スキンケア", "name": "TK ローション化粧水・ローション", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "150mL"}, {"cat": "スキンケア", "name": "TK ローション（無香料）化粧水・ローション", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "150mL"}, {"cat": "スキンケア", "name": "メリーゼ ローション化粧水・ローション", "priceEx": "5,500円", "priceTax": "6,050円", "vol": "130mL"}, {"cat": "スキンケア", "name": "デルピア Ⅱ スキンローション＜Ｍ＞化粧水・ローション", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "130mL"}, {"cat": "スキンケア", "name": "デルピア Ⅱ スキンローション＜Ｌ＞化粧水・ローション", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "130mL"}, {"cat": "スキンケア", "name": "シャンドール＜スペシャル＞化粧水・ローション", "priceEx": "2,910円", "priceTax": "3,201円", "vol": "130mL"}, {"cat": "スキンケア", "name": "シャンドール化粧水・ローション", "priceEx": "1,260円", "priceTax": "1,386円", "vol": "130mL"}, {"cat": "スキンケア", "name": "薬用 ビューネプレローション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "160mL"}, {"cat": "スキンケア", "name": "スペシャル ケアカラミン化粧水・ローション", "priceEx": "2,420円", "priceTax": "2,662円", "vol": "150mL"}, {"cat": "スキンケア", "name": "薬用ビューネ スパシャワーミスト化粧水", "priceEx": "2,800円", "priceTax": "3,080円", "vol": "60mL"}, {"cat": "スキンケア", "name": "シャルミン フレッシュナーふきとり用化粧水", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "250mL"}, {"cat": "スキンケア", "name": "シャルミンビューふきとり用化粧水", "priceEx": "2,420円", "priceTax": "2,662円", "vol": "180mL"}, {"cat": "スキンケア", "name": "ハイ・シャルミンふきとり用化粧水", "priceEx": "1,450円", "priceTax": "1,595円", "vol": "180mL"}, {"cat": "スキンケア", "name": "イルネージュ ミルク乳液・ミルクローション", "priceEx": "11,000円", "priceTax": "12,100円", "vol": "90mL"}, {"cat": "スキンケア", "name": "イルネージュ ミルク（無香料）乳液・ミルクローション", "priceEx": "11,000円", "priceTax": "12,100円", "vol": "90mL"}, {"cat": "スキンケア", "name": "リシアル ミルクローション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "100mL"}, {"cat": "スキンケア", "name": "TK ミルクローション乳液・ミルクローション", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "100mL"}, {"cat": "スキンケア", "name": "TK ミルクローション（無香料）乳液・ミルクローション", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "100mL"}, {"cat": "スキンケア", "name": "メリーゼ ミルク乳液・ミルクローション", "priceEx": "5,500円", "priceTax": "6,050円", "vol": "100mL"}, {"cat": "スキンケア", "name": "デルピア Ⅱ ミルクローション＜Ｍ＞乳液・ミルクローション", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "110mL"}, {"cat": "スキンケア", "name": "デルピア Ⅱ ミルクローション＜Ｌ＞乳液・ミルクローション", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "110mL"}, {"cat": "スキンケア", "name": "オーセント クリーム （容器付）エモリエントクリーム", "priceEx": "100,000円", "priceTax": "110,000円", "vol": "50g"}, {"cat": "スキンケア", "name": "エンベリエ ナイトクリームエモリエントクリーム", "priceEx": "35,000円", "priceTax": "38,500円", "vol": "35g"}, {"cat": "スキンケア", "name": "イルネージュ クリームエモリエントクリーム", "priceEx": "15,000円", "priceTax": "16,500円", "vol": "30g"}, {"cat": "スキンケア", "name": "イルネージュ クリーム（無香料）エモリエントクリーム", "priceEx": "15,000円", "priceTax": "16,500円", "vol": "30g"}, {"cat": "スキンケア", "name": "リシアル クリーム", "priceEx": "6,500円", "priceTax": "7,150円", "vol": "30g"}, {"cat": "スキンケア", "name": "TK クリームエモリエントクリーム", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "30g"}, {"cat": "スキンケア", "name": "TK クリーム（無香料）エモリエントクリーム", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "30g"}, {"cat": "スキンケア", "name": "メリーゼ クリームエモリエントクリーム", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "30g"}, {"cat": "スキンケア", "name": "デルピア Ⅱ クリーム＜Ｍ＞中油性エモリエントクリーム", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "40g"}, {"cat": "スキンケア", "name": "デルピア Ⅱ クリーム＜Ｌ＞弱油性エモリエントクリーム", "priceEx": "3,680円", "priceTax": "4,048円", "vol": "40g"}, {"cat": "スキンケア", "name": "エンベリエ アイクリーム", "priceEx": "24,000円", "priceTax": "26,400円", "vol": "20g"}, {"cat": "スキンケア", "name": "コラックス アイクリーム", "priceEx": "10,000円", "priceTax": "11,000円", "vol": "18g"}, {"cat": "スキンケア", "name": "マイシルバー スペリア弱油性エモリエントクリーム", "priceEx": "6,790円", "priceTax": "7,469円", "vol": "35g"}, {"cat": "スキンケア", "name": "フェアルーセント 薬用ブライトニングデイクリーム日やけ止め・デイ用クリーム", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "40g"}, {"cat": "スキンケア", "name": "オーセント ネックエッセンス首・デコルテ用美容液", "priceEx": "35,000円", "priceTax": "38,500円", "vol": "30g"}, {"cat": "スキンケア", "name": "エンベリエ エクストラクト美容液", "priceEx": "30,000円", "priceTax": "33,000円", "vol": "60mL"}, {"cat": "スキンケア", "name": "ジェネラボ エッセンス美容液", "priceEx": "12,500円", "priceTax": "13,750円", "vol": "30ｍＬ"}, {"cat": "スキンケア", "name": "フェアルーセント クリアローションふきとり用美容液", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "160mL"}, {"cat": "スキンケア", "name": "フェアルーセント 薬用ブライトニングセラム美容液", "priceEx": "10,000円", "priceTax": "11,000円", "vol": "100mL"}, {"cat": "スキンケア", "name": "薬用ラインズリセット＜４５＞（販売名：メナード 薬用エッセンスＷ）美容液", "priceEx": "20,000円", "priceTax": "22,000円", "vol": "45mL"}, {"cat": "スキンケア", "name": "薬用ラインズリセット＜２０＞（販売名：メナード 薬用エッセンスＷ）美容液", "priceEx": "10,000円", "priceTax": "11,000円", "vol": "20mL"}, {"cat": "スキンケア", "name": "コラックス美容液", "priceEx": "15,000円", "priceTax": "16,500円", "vol": "65mL"}, {"cat": "スキンケア", "name": "コラックス＜２１＞美容液", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "21mL"}, {"cat": "スキンケア", "name": "ポアライザーX美容液", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "20g"}, {"cat": "スキンケア", "name": "プログラム３０美容液", "priceEx": "20,000円", "priceTax": "22,000円", "vol": "固形パウダー：0.5ｇ×2本"}, {"cat": "スキンケア", "name": "薬用オキシコントローラー美容液", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "50mL"}, {"cat": "スキンケア", "name": "フェアルーセント 薬用ブライトニングパック", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "110g"}, {"cat": "スキンケア", "name": "ハーブマスクリームパック", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "120g"}, {"cat": "スキンケア", "name": "パック アセリエ＜クレイ＞パック", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "90g"}, {"cat": "スキンケア", "name": "オーセント マスクⅡパック", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "オーセント　マスクⅡ　1（首・デコルテ用パック）：28ｇ"}, {"cat": "スキンケア", "name": "薬用ビューネ スパマスクシート状マスク", "priceEx": "3,500円", "priceTax": "3,850円", "vol": "18mL×5枚入"}, {"cat": "スキンケア", "name": "エクストレッチマスク ６回分セットパック", "priceEx": "12,000円", "priceTax": "13,200円", "vol": "エクストレッチマスク　エッセンス：2ｇ×6本"}, {"cat": "スキンケア", "name": "エクストレッチマスク １回分用パック", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "エクストレッチマスク　エッセンス：2ｇ×1本"}, {"cat": "スキンケア", "name": "エモリエントパックセットパック ローション", "priceEx": "6,790円", "priceTax": "7,469円", "vol": "エモリエントパック：90g"}, {"cat": "スキンケア", "name": "パックセットパック ローション", "priceEx": "2,910円", "priceTax": "3,201円", "vol": "ビュートワール：6g×8個"}, {"cat": "スキンケア", "name": "ホワイティアパック", "priceEx": "1,940円", "priceTax": "2,134円", "vol": "60g"}, {"cat": "スキンケア", "name": "ワンタッチ 薬用リップトリートメント Ｃリップクリーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "3.2g"}, {"cat": "スキンケア", "name": "ワンタッチ 薬用リップトリートメント Ｎリップクリーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "3.2g"}, {"cat": "スキンケア", "name": "ジェネラボ 遺伝子検査キット＜美肌＞ 検査キット", "priceEx": "10,000円", "priceTax": "11,000円", "vol": ""}, {"cat": "メイクアップ", "name": "エンベリエ メイクアップベース＜しっとり＞メイクアップベース", "priceEx": "10,000円", "priceTax": "11,000円", "vol": "30g"}, {"cat": "メイクアップ", "name": "エンベリエ メイクアップベース＜さっぱり＞メイクアップベース", "priceEx": "10,000円", "priceTax": "11,000円", "vol": "50mL"}, {"cat": "メイクアップ", "name": "フェアルーセント 薬用メイクアップベース＜さっぱり＞メイクアップベース・化粧下地", "priceEx": "4,300円", "priceTax": "4,730円", "vol": "30mL"}, {"cat": "メイクアップ", "name": "フェアルーセント 薬用メイクアップベース＜しっとり＞メイクアップベース・化粧下地", "priceEx": "4,300円", "priceTax": "4,730円", "vol": "30g"}, {"cat": "メイクアップ", "name": "フェアルーセント 薬用ブライトニングコンシーラー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "4g"}, {"cat": "メイクアップ", "name": "ジュピエル メイクアップベース＜しっとり＞メイクアップベース・化粧下地", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "35g"}, {"cat": "メイクアップ", "name": "ジュピエル メイクアップベース＜さっぱり＞メイクアップベース・化粧下地", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "35mL"}, {"cat": "メイクアップ", "name": "ミネラル ベースクリームメイクアップベース・化粧下地", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "30g"}, {"cat": "メイクアップ", "name": "TK メイクアップベース＜しっとり＞メイクアップベース・化粧下地", "priceEx": "3,200円", "priceTax": "3,520円", "vol": "30g"}, {"cat": "メイクアップ", "name": "TK メイクアップベース＜さっぱり＞メイクアップベース・化粧下地", "priceEx": "3,200円", "priceTax": "3,520円", "vol": "30mL"}, {"cat": "メイクアップ", "name": "シャイニーアップベースメイクアップベース・化粧下地", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "30mL"}, {"cat": "メイクアップ", "name": "コントロールカラーベースメイクアップベース・化粧下地", "priceEx": "3,500円", "priceTax": "3,850円", "vol": "30g"}, {"cat": "メイクアップ", "name": "ジュピエル コントロールカラーファンデーション", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "20g"}, {"cat": "メイクアップ", "name": "ジュピエル リクイドコンシーラー", "priceEx": "4,000円", "priceTax": "4,400円", "vol": ""}, {"cat": "メイクアップ", "name": "エンベリエ パウダーファンデーション", "priceEx": "12,000円", "priceTax": "13,200円", "vol": "11.5g"}, {"cat": "メイクアップ", "name": "エンベリエ エッセンスインファンデーション", "priceEx": "15,000円", "priceTax": "16,500円", "vol": "モイストタイプ 7g ・ パウダータイプ 7.5g"}, {"cat": "メイクアップ", "name": "フェアルーセント ブライトニングファンデーション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "11g"}, {"cat": "メイクアップ", "name": "フェアルーセント ブライトニングリクイドファンデーション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "30mL"}, {"cat": "メイクアップ", "name": "ジュピエル パウダーファンデーション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "11g"}, {"cat": "メイクアップ", "name": "ジュピエル リクイドファンデーション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "30mL"}, {"cat": "メイクアップ", "name": "ミネラル ファンデーション", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "6g"}, {"cat": "メイクアップ", "name": "TK パウダーファンデーション", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "12g"}, {"cat": "メイクアップ", "name": "フェアルーセント 薬用ブライトニングプレストパウダーフェイスパウダー", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "10g"}, {"cat": "メイクアップ", "name": "ジュピエル フェイスパウダーおしろい", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "30g"}, {"cat": "メイクアップ", "name": "ジュピエル プレストパウダーおしろい", "priceEx": "6,000円", "priceTax": "6,600円", "vol": "10g"}, {"cat": "メイクアップ", "name": "TK ビューティキープ フェイスパウダーおしろい", "priceEx": "3,500円", "priceTax": "3,850円", "vol": "7g"}, {"cat": "メイクアップ", "name": "ジュピエル アイカラー＜１色入＞アイカラー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル クリーミィペイントカラーアイカラー・ほべに・おしろい・ボディ用メイクアップ", "priceEx": "3,200円", "priceTax": "3,520円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル アイカラーコンパクト ３１アイカラー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル アイカラーコンパクト ３２アイカラー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル アイカラーコンパクト ３アイカラー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル アイカラーコンパクト ３４アイカラー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル アイブロウペンシル", "priceEx": "3,500円", "priceTax": "3,850円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル パウダーブロウライナーアイブロウ・アイライナー", "priceEx": "5,500円", "priceTax": "6,050円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル パウダーアイブロウ＜１色入＞アイブロウ・眉墨", "priceEx": "1,600円", "priceTax": "1,760円", "vol": ""}, {"cat": "メイクアップ", "name": "アイライナー＜ウオータープルーフ＞アイライナー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジェルアイライナーペンシルアイライナー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "リクイドアイライナー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル マスカラベースまつ毛下地化粧料", "priceEx": "2,500円", "priceTax": "2,750円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ボリュームマスカラ", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ボリュームマスカラＷＰマスカラ", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ロングラッシュマスカラ", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ロングラッシュマスカラＷＰマスカラ", "priceEx": "3,000円", "priceTax": "3,300円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ポイントメイクリムーバーメイクアップリムーバー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "100mL"}, {"cat": "メイクアップ", "name": "エンベリエ リップスティック口紅", "priceEx": "8,000円", "priceTax": "8,800円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル ワンタッチリップスティック口紅", "priceEx": "4,700円", "priceTax": "5,170円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル リップペンシル口紅", "priceEx": "4,000円", "priceTax": "4,400円", "vol": ""}, {"cat": "メイクアップ", "name": "TK ワンタッチリップスティック口紅・リップクリーム", "priceEx": "3,200円", "priceTax": "3,520円", "vol": ""}, {"cat": "メイクアップ", "name": "TK エナメルージュ口紅・唇用美容液", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "6mL"}, {"cat": "メイクアップ", "name": "ジュピエル フェイスカラー＜１色入＞チーク（ほべに）・おしろい", "priceEx": "2,200円", "priceTax": "2,420円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル フェイスカラーコンパクト ３１ほべに", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル フェイスカラーコンパクト ３２ほべに", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル フェイスカラーコンパクト ３ほほべに・おしろい", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "メイクアップ", "name": "ジュピエル フェイスカラーコンパクト ３４ほべに・おしろい", "priceEx": "5,000円", "priceTax": "5,500円", "vol": ""}, {"cat": "ヘアケア", "name": "クロワ シャンプー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "300mL"}, {"cat": "ヘアケア", "name": "クロワ ボリュームアップシャンプー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "300mL"}, {"cat": "ヘアケア", "name": "薬用シャンプーＳＶ", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "300mL"}, {"cat": "ヘアケア", "name": "アロマローズ シャンプー", "priceEx": "1,000円", "priceTax": "1,100円", "vol": "250mL"}, {"cat": "ヘアケア", "name": "アロマローズ シャンプー＜ポンプタイプ＞", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "550mL"}, {"cat": "ヘアケア", "name": "クロワ コンディショナー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "250g"}, {"cat": "ヘアケア", "name": "クロワ ボリュームアップコンディショナー", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "250g"}, {"cat": "ヘアケア", "name": "アロマローズ コンディショナー", "priceEx": "1,000円", "priceTax": "1,100円", "vol": "250mL"}, {"cat": "ヘアケア", "name": "アロマローズ コンディショナー＜ポンプタイプ＞", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "550mL"}, {"cat": "ヘアケア", "name": "クロワ ヘヤーエステトリートメント", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "240g"}, {"cat": "ヘアケア", "name": "クロワ ヘヤーエッセンス（アウトバス）", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "150mL"}, {"cat": "ヘアケア", "name": "クロワ ブローミスト", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "200mL"}, {"cat": "ヘアケア", "name": "クロワ ボリュームアップミスト", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "150mL"}, {"cat": "ヘアケア", "name": "クロワ クリームワックス", "priceEx": "2,000円", "priceTax": "2,200円", "vol": ""}, {"cat": "ヘアケア", "name": "クロワ スタイリングフォーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": ""}, {"cat": "ヘアケア", "name": "クロワ ヘヤースプレー", "priceEx": "2,000円", "priceTax": "2,200円", "vol": ""}, {"cat": "ヘアケア", "name": "ウェーブマイド＜ハード＞", "priceEx": "1,450円", "priceTax": "1,595円", "vol": ""}, {"cat": "ヘアケア", "name": "ウェーブマイド＜ハード＞（無香料）", "priceEx": "1,500円", "priceTax": "1,650円", "vol": ""}, {"cat": "ヘアケア", "name": "ウェーブマイド＜ハード＞（無香料）＜携帯用＞", "priceEx": "", "priceTax": "", "vol": ""}, {"cat": "ヘアケア", "name": "ブリアンチン ジャコー", "priceEx": "", "priceTax": "", "vol": ""}, {"cat": "ヘアケア", "name": "ハイセス クリーミィヘヤーカラー＜エレガント＞", "priceEx": "", "priceTax": "", "vol": ""}, {"cat": "ヘアケア", "name": "ワンタッチヘヤーカラー", "priceEx": "", "priceTax": "", "vol": ""}, {"cat": "ヘアケア", "name": "薬用スカルプエッセンスＳＶ", "priceEx": "", "priceTax": "", "vol": ""}, {"cat": "フレグランス", "name": "緑映 オーデトワレ", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "50mL"}, {"cat": "フレグランス", "name": "重ね香 オーデトワレ", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "50mL"}, {"cat": "フレグランス", "name": "オーセント オーデパルファム", "priceEx": "15,000円", "priceTax": "16,500円", "vol": "50mL"}, {"cat": "フレグランス", "name": "たおや香オーデパルファム", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "50mL"}, {"cat": "ネイル", "name": "ネイルカラー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "10mL"}, {"cat": "ネイル", "name": "ネイルカラー リムーバー除光液", "priceEx": "1,000円", "priceTax": "1,100円", "vol": "100mL"}, {"cat": "ネイル", "name": "ネイルカラー うすめ液ネイルカラーうすめ液", "priceEx": "800円", "priceTax": "880円", "vol": "10mL"}, {"cat": "メンズ", "name": "デライブ クレンジングクリーム", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "130g"}, {"cat": "メンズ", "name": "フォーメン フォームウオッシング洗顔料", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "200mL"}, {"cat": "メンズ", "name": "デライブ ウオッシングクリーム 洗顔クリーム", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "130g"}, {"cat": "メンズ", "name": "フォーメン シェービングジェル", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "130g"}, {"cat": "メンズ", "name": "デライブ ローション ローション", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "160mL"}, {"cat": "メンズ", "name": "アダムス エクート スキンローション化粧水・ローション", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "160mL"}, {"cat": "メンズ", "name": "デライブ ミルクローション ミルクローション", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "80mL"}, {"cat": "メンズ", "name": "アダムス エクート 薬用クリーム弱油性クリーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "60g"}, {"cat": "メンズ", "name": "デライブ フェイスカラークリーム デイ用フェイスカラークリーム", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "30g"}, {"cat": "メンズ", "name": "ギーベル オーデコロン", "priceEx": "5,330円", "priceTax": "5,863円", "vol": "120mL"}, {"cat": "メンズ", "name": "アダムス エクート 薬用スカルプトニックヘアトニック", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "160mL"}, {"cat": "メンズ", "name": "アダムス エクート オイルフリーリキッド整髪料・ヘアリキッド", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "160mL"}, {"cat": "ボディケア", "name": "ハーティコフレ浴用化粧料、ボディ用ミルクローション、ハンド用クリーム、爪化粧料", "priceEx": "10,000円", "priceTax": "11,000円", "vol": ""}, {"cat": "ボディケア", "name": "プリメーラ石けん", "priceEx": "900円", "priceTax": "990円", "vol": "標準重量100g"}, {"cat": "ボディケア", "name": "リラタビ ボディシャンプー アーバンラグジュアリーの香りボディシャンプー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "300mL"}, {"cat": "ボディケア", "name": "リラタビ ボディシャンプー 高原の花畑の香りボディシャンプー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "300mL"}, {"cat": "ボディケア", "name": "リラタビ ボディシャンプー 青々とした竹林の香りボディシャンプー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "300mL"}, {"cat": "ボディケア", "name": "アロマローズ ボディシャンプー＜ポンプタイプ＞ボディシャンプー", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "550mL"}, {"cat": "ボディケア", "name": "シルク成分配合のボディシャンプー", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "550mL"}, {"cat": "ボディケア", "name": "緑映 パヒュームドシャワージェルボディシャンプー", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "300mL"}, {"cat": "ボディケア", "name": "重ね香 ボディエマルジョンボディ用乳液", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "200mL"}, {"cat": "ボディケア", "name": "オーセント ネックエッセンス首・デコルテ用美容液", "priceEx": "35,000円", "priceTax": "38,500円", "vol": "30g"}, {"cat": "ボディケア", "name": "クールボディエッセンスボディ用美容液", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "200mL"}, {"cat": "ボディケア", "name": "薬用クリームボディ用クリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "120g"}, {"cat": "ボディケア", "name": "セクール ボディパウダー＜プレスト＞ボディパウダー", "priceEx": "5,000円", "priceTax": "5,500円", "vol": "100g"}, {"cat": "ボディケア", "name": "アフェリー 薬用パウダー＜プレスト＞ボディパウダー", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "45g"}, {"cat": "ボディケア", "name": "緑映 パヒュームドパウダーボディ用パヒュームパウダー", "priceEx": "7,000円", "priceTax": "7,700円", "vol": "100g"}, {"cat": "ボディケア", "name": "薬用ビューネ バスソルト薬用入浴剤", "priceEx": "3,500円", "priceTax": "3,850円", "vol": "20g×30包入"}, {"cat": "ボディケア", "name": "美宝の恵み浴用化粧料", "priceEx": "3,500円", "priceTax": "3,850円", "vol": "黄：20g×（7包入×2箱）"}, {"cat": "ボディケア", "name": "薬用湯（販売名：薬用入浴剤Ｇ）薬用入浴剤", "priceEx": "3,400円", "priceTax": "3,740円", "vol": "37mLｘ20包"}, {"cat": "ボディケア", "name": "ハンドケアエッセンスハンド用美容液", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "200mL"}, {"cat": "ボディケア", "name": "バスデリラク ハンドクリーム", "priceEx": "1,500円", "priceTax": "1,650円", "vol": "80g"}, {"cat": "ボディケア", "name": "アフェリー モイスチャーハンドヴェールハンドクリーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "80g"}, {"cat": "ボディケア", "name": "ヴェール ハンドケアハンド＆ネイル用ジェル状クリーム", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "80g"}, {"cat": "ボディケア", "name": "薬用 ハンドソープ", "priceEx": "1,200円", "priceTax": "1,320円", "vol": "300mL"}, {"cat": "ボディケア", "name": "ハンドクリーナー手指用洗浄料", "priceEx": "1,800円", "priceTax": "1,980円", "vol": "250mL"}, {"cat": "ボディケア", "name": "リフレッシュボディミストボディ用ミスト状ローション", "priceEx": "2,500円", "priceTax": "2,750円", "vol": "100mL"}, {"cat": "ボディケア", "name": "ＵＶプロテクトＷＰ 日やけ止めミルクローション", "priceEx": "3,000円", "priceTax": "3,300円", "vol": "50mL"}, {"cat": "ボディケア", "name": "キッズブロックＵＶ日やけ止めクリーム", "priceEx": "2,000円", "priceTax": "2,200円", "vol": "50g"}, {"cat": "ボディケア", "name": "薬用デントバランス薬用はみがき", "priceEx": "2,800円", "priceTax": "3,080円", "vol": "125g"}, {"cat": "ボディケア", "name": "薬用デントバランス マウスウオッシュ薬用洗口液", "priceEx": "4,000円", "priceTax": "4,400円", "vol": "10ｍL×60包"}, {"cat": "健康食品", "name": "霊芝マンネンタケ（赤霊芝・黒霊芝）含有加工食品", "priceEx": "15,000円", "priceTax": "16,200円", "vol": "55.5g (555mg×100粒､1粒の内容液360mg)"}, {"cat": "健康食品", "name": "イチョウ葉・ＤＨＡイチョウ葉エキス・ＤＨＡ含有精製魚油加工食品", "priceEx": "10,000円", "priceTax": "10,800円", "vol": "55g （550mg×100粒、1粒の内容液360mg）"}, {"cat": "健康食品", "name": "グルコサミン含有加工食品", "priceEx": "6,000円", "priceTax": "6,480円", "vol": "81g （300mg×135粒×2袋）"}, {"cat": "健康食品", "name": "スタイルアシストブラックジンジャー抽出物・ターミナリアベリカ抽出物含有加工食品", "priceEx": "7,000円", "priceTax": "7,560円", "vol": "27g （300mg×45粒×2袋）"}, {"cat": "健康食品", "name": "ビフィズスプラス乳酸菌（ビフィズス生菌）含有加工食品", "priceEx": "6,000円", "priceTax": "6,480円", "vol": "45g （1.5g×30包入）"}, {"cat": "健康食品", "name": "マルチビタミン・Ｑ１０ビタミン類・コエンザイムQ10・エゾウコギエキス含有加工食品", "priceEx": "6,000円", "priceTax": "6,480円", "vol": "29g （323mg×45粒×2袋：1粒の内容物260mg）"}, {"cat": "健康食品", "name": "アイ・マルチサプリビルベリー由来アントシアニン・ルテイン・ゼアキサンチン含有加工食品", "priceEx": "5,000円", "priceTax": "5,400円", "vol": "46.2g（385mg×120粒、1粒の内容液250mg）"}, {"cat": "健康食品", "name": "オリーブ葉タブレットオリーブ葉エキス含有加工食品", "priceEx": "3,600円", "priceTax": "3,888円", "vol": "128g （1g×128個）"}, {"cat": "健康食品", "name": "カルシウム・鉄カルシウム・マグネシウム・鉄含有加工食品", "priceEx": "3,600円", "priceTax": "3,888円", "vol": "90g （500mg×60個×3袋）"}, {"cat": "健康食品", "name": "霊芝ジンジャー粉末清涼飲料", "priceEx": "3,500円", "priceTax": "3,780円", "vol": ""}, {"cat": "健康食品", "name": "フェアルーセント Ｃ・クリア・ビューティビタミンＣ含有加工食品", "priceEx": "4,500円", "priceTax": "4,860円", "vol": "96g （1.6g×60包）"}, {"cat": "健康食品", "name": "華かさね清涼飲料水", "priceEx": "10,000円", "priceTax": "10,800円", "vol": "30mL×10本"}, {"cat": "健康食品", "name": "霊芝ドリンク清涼飲料水", "priceEx": "5,000円", "priceTax": "5,400円", "vol": "30mL×10本"}, {"cat": "健康食品", "name": "健康野菜・果実ミックスジュース", "priceEx": "7,500円", "priceTax": "8,100円", "vol": "160g×30缶"}, {"cat": "健康食品", "name": "紫の健康野菜・果実ミックスジュース", "priceEx": "7,500円", "priceTax": "8,100円", "vol": "160g×30缶"}, {"cat": "健康食品", "name": "緑の健康野菜・果実ミックスジュース", "priceEx": "7,500円", "priceTax": "8,100円", "vol": "160g×30缶"}, {"cat": "健康食品", "name": "霊芝・人参Ｗゴールド清涼飲料水", "priceEx": "5,000円", "priceTax": "5,400円", "vol": "50mL×10本"}, {"cat": "健康食品", "name": "ＲＪ・マカ ゴールド清涼飲料水", "priceEx": "3,500円", "priceTax": "3,780円", "vol": "30mL×10本"}, {"cat": "健康食品", "name": "コラーゲン ゴールド清涼飲料水", "priceEx": "3,500円", "priceTax": "3,780円", "vol": "30mL×10本"}, {"cat": "健康食品", "name": "霊芝・ビネガー ＜黒酢配合＞調味酢", "priceEx": "5,000円", "priceTax": "5,400円", "vol": "500mL"}, {"cat": "健康食品", "name": "くま笹健康茶＜７５＞くま笹混合茶（ティーバッグ）", "priceEx": "3,000円", "priceTax": "3,240円", "vol": "150g （2g×75袋入）"}, {"cat": "健康食品", "name": "桑の葉健康茶＜７５＞桑の葉混合茶（ティーバッグ）", "priceEx": "3,000円", "priceTax": "3,240円", "vol": "150g （2g×75袋入）"}, {"cat": "健康食品", "name": "霊芝ウーロン茶＜７５＞ウーロン茶（ティーバッグ）", "priceEx": "3,000円", "priceTax": "3,240円", "vol": "150g （2g×75袋入）"}, {"cat": "健康食品", "name": "霊芝紅茶＜７５＞紅茶（ティーバッグ）", "priceEx": "3,000円", "priceTax": "3,240円", "vol": "165g （2.2g×75袋入）"}, {"cat": "健康食品", "name": "健康茶セット＜７５＞混合茶（ティーバッグ）詰め合わせ【ウーロン茶・くま笹混合茶・桑の葉混合茶】", "priceEx": "3,000円", "priceTax": "3,240円", "vol": ""}, {"cat": "健康食品", "name": "健康茶セット＜１３０＞混合茶（ティーバッグ）詰め合わせ【ウーロン茶・紅茶・くま笹混合茶・桑の葉混合茶】", "priceEx": "5,000円", "priceTax": "5,400円", "vol": ""}, {"cat": "化粧用具", "name": "ファンデーション＜パウダータイプ＞ パフAファンデーションパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "ファンデーション＜モイストタイプ＞ パフAファンデーションパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "パウダーファンデーション パフEファンデーションパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "パウダーファンデーション パフＷファンデーションパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "リクイドファンデーション パフＷファンデーションパフ", "priceEx": "500円", "priceTax": "550円", "vol": ""}, {"cat": "化粧用具", "name": "プレストパウダー パフＦフェイスパウダーパフ", "priceEx": "350円", "priceTax": "385円", "vol": ""}, {"cat": "化粧用具", "name": "プレストパウダー パフＪフェイスパウダーパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "パウダーファンデーション パフＡファンデーションパフ", "priceEx": "300円", "priceTax": "330円", "vol": ""}, {"cat": "化粧用具", "name": "フェイスパウダー パフＡフェイスパウダーパフ", "priceEx": "550円", "priceTax": "605円", "vol": ""}, {"cat": "化粧用具", "name": "パウダーファンデーション パフ <ホームユース>ファンデーションパフ", "priceEx": "600円", "priceTax": "660円", "vol": ""}, {"cat": "化粧用具", "name": "パウダー パフ＜ルースタイプ用＞パウダーパフ", "priceEx": "350円", "priceTax": "385円", "vol": ""}, {"cat": "化粧用具", "name": "ジュピエル ポイントメイク ケース（ゴールド）メイクアップパレットケース", "priceEx": "1,000円", "priceTax": "1,100円", "vol": ""}, {"cat": "化粧用具", "name": "ジュピエル ポイントメイク ケース（シルバー）メイクアップパレットケース", "priceEx": "1,000円", "priceTax": "1,100円", "vol": ""}, {"cat": "化粧用具", "name": "ジュピエル ビューティパレット ケースＤメイクアップパレットケース", "priceEx": "1,500円", "priceTax": "1,650円", "vol": ""}, {"cat": "化粧用具", "name": "アイカラー ブラシセットＢアイカラーブラシ&チップ", "priceEx": "500円", "priceTax": "550円", "vol": ""}, {"cat": "化粧用具", "name": "フェイスカラー ブラッシュＢフェイスカラーブラシ", "priceEx": "550円", "priceTax": "605円", "vol": ""}, {"cat": "化粧用具", "name": "フェイスカラー ブラッシュＪフェイスカラーブラシ", "priceEx": "550円", "priceTax": "605円", "vol": ""}, {"cat": "化粧用具", "name": "アイカラー ブラッシュＪアイカラーブラシ&チップ", "priceEx": "350円", "priceTax": "385円", "vol": ""}, {"cat": "化粧用具", "name": "リップカラー ブラッシュリップブラシ", "priceEx": "700円", "priceTax": "770円", "vol": ""}, {"cat": "化粧用具", "name": "パウダーブロウライナー ブラシセットJアイメイクブラシ&チップ", "priceEx": "600円", "priceTax": "660円", "vol": ""}, {"cat": "化粧用具", "name": "アイカラーブラッシュアイカラーブラシ", "priceEx": "800円", "priceTax": "880円", "vol": ""}, {"cat": "化粧用具", "name": "アイカラーチップ（チップスペアー2本付）アイカラーチップ", "priceEx": "800円", "priceTax": "880円", "vol": ""}];

let currentCat = 'すべて';
let searchText = '';
let orderItems = []; // [{product, qty}]

// カテゴリ一覧
const cats = ['すべて', ...new Set(PRODUCTS.map(p => p.cat))];

// 初期化
function init() {
  // 今日の日付セット
  document.getElementById('orderDate').value = new Date().toISOString().split('T')[0];

  // カテゴリタブ
  const tabsEl = document.getElementById('catTabs');
  cats.forEach(cat => {
    const btn = document.createElement('button');
    btn.className = 'cat-btn' + (cat === 'すべて' ? ' active' : '');
    btn.textContent = cat;
    btn.onclick = () => selectCat(cat);
    tabsEl.appendChild(btn);
  });

  // 検索
  document.getElementById('searchInput').addEventListener('input', e => {
    searchText = e.target.value;
    renderProducts();
  });

  renderProducts();
}

function selectCat(cat) {
  currentCat = cat;
  document.querySelectorAll('.cat-btn').forEach(b => {
    b.classList.toggle('active', b.textContent === cat);
  });
  renderProducts();
}

function renderProducts() {
  const list = document.getElementById('productList');
  const filtered = PRODUCTS.filter(p => {
    const catOk = currentCat === 'すべて' || p.cat === currentCat;
    const searchOk = !searchText || p.name.toLowerCase().includes(searchText.toLowerCase());
    return catOk && searchOk;
  });

  list.innerHTML = filtered.map((p, i) => {
    const idx = PRODUCTS.indexOf(p);
    const displayPrice = p.priceTax || p.priceEx || '価格不明';
    const volText = p.vol ? `<span class="prod-vol">${p.vol}</span>` : '';
    return `<div class="product-item" onclick="addProduct(${idx})">
      <span class="cat-badge">${p.cat}</span>
      <span class="prod-name">${p.name}</span>
      ${volText}
      <span class="prod-price">${displayPrice}</span>
      <button class="add-btn" onclick="event.stopPropagation();addProduct(${idx})">+</button>
    </div>`;
  }).join('');

  if (filtered.length === 0) {
    list.innerHTML = '<div class="order-empty">商品が見つかりません</div>';
  }
}

function addProduct(idx) {
  const p = PRODUCTS[idx];
  const existing = orderItems.find(o => o.product === p);
  if (existing) {
    existing.qty++;
  } else {
    orderItems.push({ product: p, qty: 1 });
  }
  renderOrder();
}

function changeQty(i, delta) {
  orderItems[i].qty += delta;
  if (orderItems[i].qty <= 0) {
    orderItems.splice(i, 1);
  }
  renderOrder();
}

function removeItem(i) {
  orderItems.splice(i, 1);
  renderOrder();
}

function parsePrice(str) {
  if (!str) return 0;
  const m = str.match(/[\d,]+/);
  return m ? parseInt(m[0].replace(/,/g, '')) : 0;
}

function formatNum(n) {
  return n.toLocaleString('ja-JP') + '円';
}

function renderOrder() {
  const el = document.getElementById('orderItems');
  if (orderItems.length === 0) {
    el.innerHTML = '<div class="order-empty">← 左の商品を選んでください</div>';
    document.getElementById('subtotalEx').textContent = '0円';
    document.getElementById('subtotalTax').textContent = '0円';
    document.getElementById('grandTotal').textContent = '0円';
    return;
  }

  let totalEx = 0, totalTax = 0;

  el.innerHTML = orderItems.map((o, i) => {
    const exPrice = parsePrice(o.product.priceEx);
    const taxPrice = parsePrice(o.product.priceTax) || exPrice;
    const subtax = taxPrice * o.qty;
    totalEx += exPrice * o.qty;
    totalTax += taxPrice * o.qty;
    return `<div class="order-item">
      <span class="o-name">${o.product.name}</span>
      <div class="qty-ctrl">
        <button class="qty-btn" onclick="changeQty(${i},-1)">−</button>
        <span class="qty-num">${o.qty}</span>
        <button class="qty-btn" onclick="changeQty(${i},1)">＋</button>
      </div>
      <span class="o-subtotal">${formatNum(subtax)}</span>
      <button class="del-btn" onclick="removeItem(${i})">×</button>
    </div>`;
  }).join('');

  document.getElementById('subtotalEx').textContent = formatNum(totalEx);
  document.getElementById('subtotalTax').textContent = formatNum(totalTax);
  document.getElementById('grandTotal').textContent = formatNum(totalTax);
}

function clearOrder() {
  if (orderItems.length === 0) return;
  if (confirm('注文をクリアしますか？')) {
    orderItems = [];
    document.getElementById('customerName').value = '';
    document.getElementById('orderNote').value = '';
    document.getElementById('orderDate').value = new Date().toISOString().split('T')[0];
    renderOrder();
  }
}

function saveOrder() {
  if (orderItems.length === 0) { alert('注文商品がありません'); return; }
  const name = document.getElementById('customerName').value || '（名前未入力）';
  const date = document.getElementById('orderDate').value;
  const note = document.getElementById('orderNote').value;

  let totalTax = 0;
  const items = orderItems.map(o => {
    const tax = parsePrice(o.product.priceTax) || parsePrice(o.product.priceEx);
    totalTax += tax * o.qty;
    return { name: o.product.name, priceEx: o.product.priceEx, priceTax: o.product.priceTax, qty: o.qty };
  });

  const order = { id: Date.now(), date, customer: name, note, items, totalTax };
  const history = JSON.parse(localStorage.getItem('menardOrders') || '[]');
  history.unshift(order);
  localStorage.setItem('menardOrders', JSON.stringify(history));
  alert(`「${name}」様の注文を保存しました！`);
}

function printOrder() {
  if (orderItems.length === 0) { alert('注文商品がありません'); return; }
  window.print();
}

function sendToLINE() {
  if (orderItems.length === 0) { alert('注文商品がありません'); return; }
  const name = document.getElementById('customerName').value || '（名前未入力）';
  const date = document.getElementById('orderDate').value;
  const note = document.getElementById('orderNote').value;
  let totalTax = 0;
  const lines = orderItems.map(o => {
    const tax = parsePrice(o.product.priceTax) || parsePrice(o.product.priceEx);
    totalTax += tax * o.qty;
    return `・${o.product.name}　×${o.qty}　${formatNum(tax * o.qty)}`;
  });
  const text = [
    '🌸 メナード注文',
    `━━━━━━━━━━`,
    `👤 お名前：${name}`,
    `📅 日付：${date}`,
    note ? `📝 メモ：${note}` : '',
    `━━━━━━━━━━`,
    ...lines,
    `━━━━━━━━━━`,
    `💴 合計（税込）：${formatNum(totalTax)}`,
  ].filter(Boolean).join('\n');

  // クリップボードにコピー
  navigator.clipboard.writeText(text).then(() => {
    // LINEを開く（公式アカウントのチャット画面）
    const encoded = encodeURIComponent(text);
    window.open(`https://line.me/R/oaMessage/@761sosfz/?text=${encoded}`, '_blank');
  }).catch(() => {
    // クリップボード失敗時はアラートで表示
    alert('注文内容をコピーしてLINEで送ってください：\n\n' + text);
  });
}

function showHistory() {
  const modal = document.getElementById('historyModal');
  const body = document.getElementById('historyBody');
  const history = JSON.parse(localStorage.getItem('menardOrders') || '[]');

  if (history.length === 0) {
    body.innerHTML = '<div class="no-history">保存された注文はありません</div>';
  } else {
    body.innerHTML = history.map((o, i) => {
      const itemSummary = o.items.slice(0,3).map(it => `${it.name}×${it.qty}`).join('、');
      const more = o.items.length > 3 ? `他${o.items.length-3}点` : '';
      return `<div class="history-entry" onclick="loadOrder(${i})">
        <div class="h-meta">${o.date} ${o.note ? '📝'+o.note : ''}</div>
        <div class="h-customer">👤 ${o.customer}</div>
        <div class="h-total">合計 ${o.totalTax.toLocaleString()}円（税込）</div>
        <div class="h-items">${itemSummary}${more ? '、'+more : ''}</div>
      </div>`;
    }).join('');
  }

  modal.classList.add('show');
}

function loadOrder(i) {
  const history = JSON.parse(localStorage.getItem('menardOrders') || '[]');
  const o = history[i];
  if (!confirm(`「${o.customer}」様の注文を読み込みますか？
（現在の注文はクリアされます）`)) return;

  orderItems = o.items.map(it => {
    const product = PRODUCTS.find(p => p.name === it.name) || { ...it, cat: '' };
    return { product, qty: it.qty };
  });
  document.getElementById('customerName').value = o.customer;
  document.getElementById('orderDate').value = o.date;
  document.getElementById('orderNote').value = o.note || '';
  renderOrder();
  closeHistory();
}

function closeHistory() {
  document.getElementById('historyModal').classList.remove('show');
}

// モーダル外クリックで閉じる
document.getElementById('historyModal').addEventListener('click', function(e) {
  if (e.target === this) closeHistory();
});
[admin.html](https://github.com/user-attachments/files/26505280/admin.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>管理者ページ | MENARD おとりあ</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'Hiragino Sans', 'Yu Gothic', sans-serif; background: #fdf6f9; color: #333; font-size: 14px; }

/* ログイン画面 */
.login-wrap { display: flex; align-items: center; justify-content: center; min-height: 100vh; background: linear-gradient(135deg, #c0607a, #e8a0b0); }
.login-box { background: white; border-radius: 16px; padding: 40px 32px; width: 320px; box-shadow: 0 8px 30px rgba(0,0,0,0.15); text-align: center; }
.login-box h1 { color: #c0607a; font-size: 20px; margin-bottom: 6px; }
.login-box p { color: #999; font-size: 12px; margin-bottom: 24px; }
.login-box input { width: 100%; padding: 12px; border: 2px solid #e8c0cc; border-radius: 8px; font-size: 15px; margin-bottom: 16px; outline: none; text-align: center; letter-spacing: 2px; }
.login-box input:focus { border-color: #c0607a; }
.login-box button { width: 100%; padding: 12px; background: linear-gradient(135deg, #c0607a, #e8a0b0); color: white; border: none; border-radius: 8px; font-size: 15px; font-weight: bold; cursor: pointer; }
.login-box button:hover { opacity: 0.9; }
.login-error { color: #e00; font-size: 13px; margin-top: 10px; display: none; }

/* メイン */
.main { display: none; }
.header { background: linear-gradient(135deg, #c0607a, #e8a0b0); color: white; padding: 12px 20px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
.header h1 { font-size: 17px; }
.header-right { display: flex; gap: 10px; align-items: center; }
.logout-btn { background: rgba(255,255,255,0.25); border: none; color: white; padding: 6px 14px; border-radius: 20px; cursor: pointer; font-size: 13px; }

/* ダッシュボード */
.dashboard { padding: 16px; }
.stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-bottom: 20px; }
.stat-card { background: white; border-radius: 10px; padding: 14px; border-left: 4px solid #c0607a; box-shadow: 0 2px 8px rgba(0,0,0,0.07); }
.stat-card .num { font-size: 26px; font-weight: bold; color: #c0607a; }
.stat-card .label { font-size: 12px; color: #888; margin-top: 4px; }

/* コントロールバー */
.controls { display: flex; gap: 10px; margin-bottom: 14px; flex-wrap: wrap; align-items: center; }
.controls input, .controls select { padding: 8px 12px; border: 1.5px solid #e0c0cc; border-radius: 8px; font-size: 13px; outline: none; }
.controls input { flex: 1; min-width: 160px; }
.controls input:focus, .controls select:focus { border-color: #c0607a; }
.btn { padding: 9px 16px; border: none; border-radius: 8px; cursor: pointer; font-size: 13px; font-weight: bold; }
.btn-pink { background: linear-gradient(135deg, #c0607a, #e8a0b0); color: white; }
.btn-pink:hover { opacity: 0.9; }
.btn-green { background: #4a9060; color: white; }
.btn-green:hover { opacity: 0.9; }
.btn-gray { background: #eee; color: #666; }

/* 注文テーブル */
.order-table { width: 100%; border-collapse: collapse; background: white; border-radius: 10px; overflow: hidden; box-shadow: 0 2px 8px rgba(0,0,0,0.07); }
.order-table th { background: #c0607a; color: white; padding: 12px 10px; text-align: left; font-size: 13px; white-space: nowrap; }
.order-table td { padding: 10px; border-bottom: 1px solid #f0e0e8; font-size: 13px; vertical-align: top; }
.order-table tr:last-child td { border-bottom: none; }
.order-table tr:hover td { background: #fff5f8; }

/* ステータスバッジ */
.badge { display: inline-block; padding: 3px 10px; border-radius: 12px; font-size: 12px; font-weight: bold; white-space: nowrap; }
.badge-new    { background: #ffe0e6; color: #c0607a; }
.badge-proc   { background: #fff0d0; color: #c06820; }
.badge-ship   { background: #d0e8ff; color: #2060c0; }
.badge-done   { background: #d0f0e0; color: #207040; }

/* アクションボタン */
.act-btn { padding: 4px 10px; border: none; border-radius: 6px; cursor: pointer; font-size: 12px; margin: 2px; }
.act-edit { background: #e8f0ff; color: #2060c0; }
.act-del  { background: #ffe8e8; color: #c02020; }
.act-stat { background: #f0f0f0; color: #444; }

/* モーダル */
.modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 200; align-items: center; justify-content: center; }
.modal.show { display: flex; }
.modal-box { background: white; border-radius: 14px; padding: 24px; width: 95%; max-width: 520px; max-height: 90vh; overflow-y: auto; }
.modal-box h2 { color: #c0607a; font-size: 16px; margin-bottom: 18px; }
.form-group { margin-bottom: 14px; }
.form-group label { display: block; font-size: 12px; color: #888; margin-bottom: 4px; font-weight: bold; }
.form-group input, .form-group select, .form-group textarea { width: 100%; padding: 9px 12px; border: 1.5px solid #e0c0cc; border-radius: 8px; font-size: 14px; outline: none; }
.form-group input:focus, .form-group select:focus, .form-group textarea:focus { border-color: #c0607a; }
.form-group textarea { min-height: 80px; resize: vertical; }
.modal-footer { display: flex; gap: 10px; margin-top: 20px; justify-content: flex-end; }

/* 注文商品リスト（モーダル内） */
.items-area { border: 1.5px solid #e0c0cc; border-radius: 8px; padding: 10px; margin-top: 4px; max-height: 200px; overflow-y: auto; }
.item-row { display: flex; gap: 8px; margin-bottom: 6px; align-items: center; }
.item-row input { flex: 1; }
.item-row input.qty { width: 60px; flex: none; }
.item-row .rm { background: none; border: none; color: #ccc; cursor: pointer; font-size: 18px; }
.item-row .rm:hover { color: #e00; }
.add-item-btn { color: #c0607a; background: none; border: 1px dashed #c0607a; border-radius: 6px; padding: 5px 12px; cursor: pointer; font-size: 12px; margin-top: 6px; }

/* 空状態 */
.empty-state { text-align: center; padding: 40px; color: #ccc; }

@media (max-width: 600px) {
  .stats { grid-template-columns: repeat(2, 1fr); }
  .order-table { font-size: 12px; }
  .order-table th, .order-table td { padding: 7px 6px; }
}
</style>
</head>
<body>

<!-- ログイン画面 -->
<div class="login-wrap" id="loginWrap">
  <div class="login-box">
    <h1>🌸 管理者ログイン</h1>
    <p>MENARD おとりあ</p>
    <input type="password" id="pwInput" placeholder="パスワードを入力" onkeydown="if(event.key==='Enter')login()">
    <button onclick="login()">ログイン</button>
    <div class="login-error" id="loginError">パスワードが違います</div>
  </div>
</div>

<!-- メイン管理画面 -->
<div class="main" id="mainWrap">
  <div class="header">
    <h1>🌸 MENARD おとりあ 管理者ページ</h1>
    <div class="header-right">
      <span id="todayDate" style="font-size:13px;opacity:0.9"></span>
      <button class="logout-btn" onclick="logout()">ログアウト</button>
    </div>
  </div>

  <div class="dashboard">
    <!-- 統計 -->
    <div class="stats">
      <div class="stat-card">
        <div class="num" id="stat-all">0</div>
        <div class="label">📦 総注文数</div>
      </div>
      <div class="stat-card" style="border-color:#e08020">
        <div class="num" id="stat-new" style="color:#e08020">0</div>
        <div class="label">🆕 未対応</div>
      </div>
      <div class="stat-card" style="border-color:#2060c0">
        <div class="num" id="stat-ship" style="color:#2060c0">0</div>
        <div class="label">🚚 発送済み</div>
      </div>
      <div class="stat-card" style="border-color:#207040">
        <div class="num" id="stat-sales" style="color:#207040">0</div>
        <div class="label">💴 今月売上(税込)</div>
      </div>
    </div>

    <!-- コントロール -->
    <div class="controls">
      <input type="text" id="searchBox" placeholder="🔍 お客様名・商品で検索..." oninput="renderOrders()">
      <select id="statusFilter" onchange="renderOrders()">
        <option value="">すべて</option>
        <option value="未対応">🆕 未対応</option>
        <option value="処理中">⚙️ 処理中</option>
        <option value="発送済み">🚚 発送済み</option>
        <option value="完了">✅ 完了</option>
      </select>
      <button class="btn btn-pink" onclick="openAddModal()">＋ 注文を追加</button>
      <button class="btn btn-green" onclick="exportCSV()">📥 CSV出力</button>
    </div>

    <!-- 注文テーブル -->
    <div style="overflow-x:auto">
      <table class="order-table">
        <thead>
          <tr>
            <th>日付</th>
            <th>お客様名</th>
            <th>注文内容</th>
            <th>合計(税込)</th>
            <th>ステータス</th>
            <th>メモ</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody id="orderTableBody"></tbody>
      </table>
    </div>
    <div class="empty-state" id="emptyState" style="display:none">まだ注文がありません</div>
  </div>
</div>

<!-- 注文追加/編集モーダル -->
<div class="modal" id="orderModal">
  <div class="modal-box">
    <h2 id="modalTitle">＋ 注文を追加</h2>
    <div class="form-group">
      <label>お客様名 *</label>
      <input type="text" id="f-customer" placeholder="山田 花子">
    </div>
    <div class="form-group">
      <label>注文日 *</label>
      <input type="date" id="f-date">
    </div>
    <div class="form-group">
      <label>注文商品</label>
      <div class="items-area" id="itemsArea"></div>
      <button class="add-item-btn" onclick="addItemRow()">＋ 商品を追加</button>
    </div>
    <div class="form-group">
      <label>合計金額（税込）</label>
      <input type="number" id="f-total" placeholder="例：5500">
    </div>
    <div class="form-group">
      <label>ステータス</label>
      <select id="f-status">
        <option value="未対応">🆕 未対応</option>
        <option value="処理中">⚙️ 処理中</option>
        <option value="発送済み">🚚 発送済み</option>
        <option value="完了">✅ 完了</option>
      </select>
    </div>
    <div class="form-group">
      <label>配送方法</label>
      <select id="f-delivery">
        <option value="手渡し">🤝 手渡し</option>
        <option value="配送">📦 配送</option>
      </select>
    </div>
    <div class="form-group">
      <label>メモ</label>
      <textarea id="f-note" placeholder="住所・特記事項など"></textarea>
    </div>
    <div class="modal-footer">
      <button class="btn btn-gray" onclick="closeModal()">キャンセル</button>
      <button class="btn btn-pink" onclick="saveOrder()">保存する</button>
    </div>
  </div>
</div>

<script>
const PASS = '7415369mA';
let orders = [];
let editingId = null;

// ---- ログイン ----
function login() {
  const v = document.getElementById('pwInput').value;
  if (v === PASS) {
    document.getElementById('loginWrap').style.display = 'none';
    document.getElementById('mainWrap').style.display = 'block';
    sessionStorage.setItem('adminAuth', '1');
    init();
  } else {
    document.getElementById('loginError').style.display = 'block';
    document.getElementById('pwInput').value = '';
  }
}
function logout() {
  sessionStorage.removeItem('adminAuth');
  location.reload();
}
// セッション維持
if (sessionStorage.getItem('adminAuth') === '1') {
  document.getElementById('loginWrap').style.display = 'none';
  document.getElementById('mainWrap').style.display = 'block';
  init();
}

function init() {
  orders = JSON.parse(localStorage.getItem('adminOrders') || '[]');
  document.getElementById('todayDate').textContent = new Date().toLocaleDateString('ja-JP');
  renderOrders();
  updateStats();
}

// ---- 統計 ----
function updateStats() {
  document.getElementById('stat-all').textContent = orders.length;
  document.getElementById('stat-new').textContent = orders.filter(o=>o.status==='未対応').length;
  document.getElementById('stat-ship').textContent = orders.filter(o=>o.status==='発送済み').length;
  const now = new Date();
  const thisMonth = orders.filter(o => {
    const d = new Date(o.date);
    return d.getFullYear()===now.getFullYear() && d.getMonth()===now.getMonth();
  });
  const sales = thisMonth.reduce((s,o)=>s+(parseInt(o.total)||0), 0);
  document.getElementById('stat-sales').textContent = sales.toLocaleString()+'円';
}

// ---- 注文一覧 ----
const STATUS_BADGE = {
  '未対応': 'badge-new',
  '処理中': 'badge-proc',
  '発送済み': 'badge-ship',
  '完了': 'badge-done',
};
function renderOrders() {
  const search = document.getElementById('searchBox').value.toLowerCase();
  const sf = document.getElementById('statusFilter').value;
  let filtered = orders.filter(o => {
    const matchSearch = !search ||
      o.customer.toLowerCase().includes(search) ||
      (o.items||[]).some(it=>it.name.toLowerCase().includes(search));
    const matchStatus = !sf || o.status === sf;
    return matchSearch && matchStatus;
  });
  // 新しい順
  filtered = [...filtered].sort((a,b)=>new Date(b.date)-new Date(a.date));

  const tbody = document.getElementById('orderTableBody');
  const empty = document.getElementById('emptyState');
  if (filtered.length === 0) {
    tbody.innerHTML = '';
    empty.style.display = 'block';
    return;
  }
  empty.style.display = 'none';
  tbody.innerHTML = filtered.map(o => {
    const itemsText = (o.items||[]).map(it=>`${it.name}×${it.qty}`).join('、') || o.itemsText || '―';
    const badge = STATUS_BADGE[o.status] || 'badge-new';
    return `<tr>
      <td style="white-space:nowrap">${o.date}</td>
      <td style="font-weight:bold">${o.customer}</td>
      <td style="max-width:220px;word-break:break-all;font-size:12px">${itemsText}</td>
      <td style="white-space:nowrap;font-weight:bold;color:#c0607a">${parseInt(o.total||0).toLocaleString()}円</td>
      <td><span class="badge ${badge}">${o.status||'未対応'}</span></td>
      <td style="font-size:12px;color:#888">${o.note||''}<br><span style="color:#2060c0">${o.delivery||''}</span></td>
      <td style="white-space:nowrap">
        <button class="act-btn act-edit" onclick="openEditModal('${o.id}')">✏️編集</button>
        <button class="act-btn act-stat" onclick="nextStatus('${o.id}')">▶ 次へ</button>
        <button class="act-btn act-del" onclick="deleteOrder('${o.id}')">🗑</button>
      </td>
    </tr>`;
  }).join('');
}

// ---- ステータス順送り ----
const STATUS_SEQ = ['未対応','処理中','発送済み','完了'];
function nextStatus(id) {
  const o = orders.find(x=>x.id===id);
  if (!o) return;
  const idx = STATUS_SEQ.indexOf(o.status);
  o.status = STATUS_SEQ[(idx+1) % STATUS_SEQ.length];
  saveAll();
}

// ---- 削除 ----
function deleteOrder(id) {
  if (!confirm('この注文を削除しますか？')) return;
  orders = orders.filter(o=>o.id!==id);
  saveAll();
}

// ---- モーダル ----
function openAddModal() {
  editingId = null;
  document.getElementById('modalTitle').textContent = '＋ 注文を追加';
  document.getElementById('f-customer').value = '';
  document.getElementById('f-date').value = new Date().toISOString().split('T')[0];
  document.getElementById('f-total').value = '';
  document.getElementById('f-status').value = '未対応';
  document.getElementById('f-delivery').value = '手渡し';
  document.getElementById('f-note').value = '';
  document.getElementById('itemsArea').innerHTML = '';
  addItemRow();
  document.getElementById('orderModal').classList.add('show');
}
function openEditModal(id) {
  const o = orders.find(x=>x.id===id);
  if (!o) return;
  editingId = id;
  document.getElementById('modalTitle').textContent = '✏️ 注文を編集';
  document.getElementById('f-customer').value = o.customer;
  document.getElementById('f-date').value = o.date;
  document.getElementById('f-total').value = o.total || '';
  document.getElementById('f-status').value = o.status || '未対応';
  document.getElementById('f-delivery').value = o.delivery || '手渡し';
  document.getElementById('f-note').value = o.note || '';
  const area = document.getElementById('itemsArea');
  area.innerHTML = '';
  (o.items||[{name:'',qty:1}]).forEach(it=>addItemRow(it.name,it.qty));
  document.getElementById('orderModal').classList.add('show');
}
function closeModal() {
  document.getElementById('orderModal').classList.remove('show');
}

function addItemRow(name='', qty=1) {
  const area = document.getElementById('itemsArea');
  const div = document.createElement('div');
  div.className = 'item-row';
  div.innerHTML = `<input type="text" placeholder="商品名" value="${name}">
    <input type="number" class="qty" placeholder="個数" value="${qty}" min="1">
    <button class="rm" onclick="this.parentElement.remove()">×</button>`;
  area.appendChild(div);
}

function saveOrder() {
  const customer = document.getElementById('f-customer').value.trim();
  if (!customer) { alert('お客様名を入力してください'); return; }
  const rows = document.querySelectorAll('#itemsArea .item-row');
  const items = [...rows].map(r => ({
    name: r.querySelector('input[type=text]').value.trim(),
    qty: parseInt(r.querySelector('input[type=number]').value)||1,
  })).filter(it=>it.name);
  const order = {
    id: editingId || Date.now().toString(),
    customer,
    date: document.getElementById('f-date').value,
    items,
    total: document.getElementById('f-total').value,
    status: document.getElementById('f-status').value,
    delivery: document.getElementById('f-delivery').value,
    note: document.getElementById('f-note').value,
  };
  if (editingId) {
    const idx = orders.findIndex(o=>o.id===editingId);
    orders[idx] = order;
  } else {
    orders.unshift(order);
  }
  saveAll();
  closeModal();
}

function saveAll() {
  localStorage.setItem('adminOrders', JSON.stringify(orders));
  renderOrders();
  updateStats();
}

// ---- CSV出力 ----
function exportCSV() {
  const bom = '\uFEFF';
  const header = 'ID,日付,お客様名,注文商品,合計(税込),ステータス,配送方法,メモ';
  const rows = orders.map(o => {
    const items = (o.items||[]).map(it=>`${it.name}×${it.qty}`).join('・');
    return [o.id,o.date,o.customer,items,o.total||'',o.status||'',o.delivery||'',o.note||'']
      .map(v=>`"${(v+'').replace(/"/g,'""')}"`).join(',');
  });
  const csv = bom + header + '\n' + rows.join('\n');
  const a = document.createElement('a');
  a.href = 'data:text/csv;charset=utf-8,' + encodeURIComponent(csv);
  a.download = `メナード注文_${new Date().toISOString().split('T')[0]}.csv`;
  a.click();
}

// モーダル外クリックで閉じる
document.getElementById('orderModal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});
</script>
</body>
</html>

init();
</script>
</body>
</html>
