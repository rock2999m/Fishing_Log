# 🎣 Fishing Trip Memory

釣行記録アプリ - 釣果、潮汐、天気を自動で記録できるPWA（Progressive Web App）

## ✨ 機能

- **釣行記録**: 開始時刻、終了時刻、位置情報を自動記録
- **釣果管理**: 釣った魚の種類、サイズ、体重、釣法を記録
- **自動データ取得**:
  - 🌦️ 天気情報（OpenWeatherMap API）
  - 🌊 潮汐情報（JHA 潮汐データベース）
  - 💧 水温（MIRC 提供）
- **潮汐画像**: 釣行時間帯の潮汐グラフを自動取得
- **オフライン対応**: Service Worker でオフライン時も利用可能
- **データ永続化**: LocalStorage + IndexedDB でデータ保護
- **PWA対応**: ホーム画面にインストール可能

## 🚀 クイックスタート

### 1. アイコンの準備

提供されたアイコン画像をリサイズ：

```bash
python resize_icons.py icon-512.png
```

生成されるファイル：
- `icon-192.png`
- `icon-512.png`
- `icon-maskable-192.png`
- `icon-maskable-512.png`

### 2. ローカル実行

```bash
# Web サーバー起動
python -m http.server 8000

# ブラウザでアクセス
http://localhost:8000/fishing\ trip\ memory.html
```

### 3. iPhone にインストール

1. Safari で上記 URL にアクセス
2. **共有** → **ホーム画面に追加**
3. **追加** をタップ

### 4. Android にインストール

1. Chrome で上記 URL にアクセス
2. **メニュー** → **アプリをインストール**
3. **インストール** をタップ

## 📁 ファイル構成

```
fishing-trip-memory/
├── fishing trip memory.html       # メインアプリケーション
├── manifest.json                  # PWA マニフェスト
├── sw.js                          # Service Worker
├── GAS_dopost                     # Google Apps Script（バックエンド）
├── icon-192.png                   # アイコン（192x192）
├── icon-512.png                   # アイコン（512x512）
├── icon-maskable-192.png          # 適応アイコン（192x192）
├── icon-maskable-512.png          # 適応アイコン（512x512）
├── resize_icons.py                # アイコンリサイザー
├── README.md                      # このファイル
├── docs/
│   ├── SETUP.md                   # 詳細セットアップガイド
│   ├── START_HERE.md              # クイックスタート
│   ├── README_ICONS.md            # アイコンガイド
│   ├── GITHUB_PUSH.md             # GitHub プッシュガイド
│   └── QUICK_SETUP.md             # クイック実行ガイド
└── .gitignore                     # Git 除外ファイル設定
```

## 🛠️ 技術スタック

### フロントエンド
- **フレームワーク**: Vanilla JavaScript (PWA)
- **UI**: Tailwind CSS
- **ストレージ**: LocalStorage + IndexedDB
- **Service Worker**: キャッシュ管理、オフライン対応

### バックエンド
- **Google Apps Script** (GAS): サーバーサイド処理
- **Google Sheets**: データ永続化
- **Google Drive**: 画像ストレージ

### 外部API
- **OpenWeatherMap**: 天気情報
- **JHA**: 潮汐データ
- **MIRC**: 水温・潮汐画像

## 🔧 セットアップ詳細

### 必要な環境
- **ブラウザ**: iOS Safari 11.3+, Chrome (Android), Edge
- **Python**: 3.7以上（ローカル実行時）
- **git**: ソースコード管理用

### インストール手順

```bash
# 1. リポジトリをクローン
git clone https://github.com/your-username/fishing-trip-memory.git
cd fishing-trip-memory

# 2. アイコンを生成
python resize_icons.py icon-512.png

# 3. Web サーバーで実行
python -m http.server 8000
```

### Google Apps Script の設定

1. [Google Apps Script](https://script.google.com) でプロジェクト作成
2. `GAS_dopost` をコピーペースト
3. 必要な設定を環境に合わせて変更
4. Web Apps としてデプロイ
5. GAS エンドポイント URL をHTMLに設定

詳細は `docs/SETUP.md` を参照してください。

## 📱 PWA 機能

### インストール
- ✅ iPhone: Safari → 共有 → ホーム画面に追加
- ✅ Android: Chrome → メニュー → アプリをインストール

### オフライン対応
- キャッシュ: Service Worker がアセット（HTML, CSS, JS）をキャッシュ
- データ: IndexedDB がドラフトデータを保存
- 同期: 接続復帰時に自動的にサーバーと同期

### ストレージ
- **LocalStorage**: 同期的なドラフト保存
- **IndexedDB**: 非同期バックアップ、より大容量

## 🎨 UI デザイン

### カラースキーム
- **背景**: Slate 950 (#0f172a)
- **釣行**: Amber
- **釣果**: Sky
- **アクセント**: Cyan

### レイアウト
1. **ホーム画面**: 釣行一覧、統計情報
2. **記録画面**: 釣行データ、天気、潮汐の入力
3. **確認画面**: 過去の釣行履歴

## 📊 データフロー

```
┌─────────────────────────────────────────────┐
│         Fishing Trip Memory App             │
├─────────────────────────────────────────────┤
│  LocalStorage ←→ IndexedDB (オフライン)     │
│         ↓ (ネットワーク時)                  │
│  GAS エンドポイント (Node.js / Node.ts)     │
│         ↓                                    │
│  Google Sheets (データベース)                │
│  Google Drive (画像ストレージ)               │
└─────────────────────────────────────────────┘

外部API連携
├─ OpenWeatherMap (天気)
├─ JHA (潮汐データ)
└─ MIRC (水温、潮汐画像)
```

## 🐛 トラブルシューティング

### アイコンが表示されない
1. アイコンファイルが正しく配置されているか確認
2. manifest.json で相対パス `./` を使用しているか確認
3. ブラウザキャッシュをクリア

詳細は `docs/README_ICONS.md` を参照。

### Service Worker が登録されない
1. HTTPS または localhost で実行しているか確認
2. manifest.json が正しく読み込まれているか確認（DevTools → Application）
3. Service Worker エラーをコンソールで確認

### オフラインで動作しない
1. Service Worker が登録されているか確認
2. IndexedDB にデータがあるか確認
3. 初回起動時は接続状態で実行

## 📚 ドキュメント

| ドキュメント | 説明 |
|------------|------|
| `docs/START_HERE.md` | 最速セットアップ（3ステップ） |
| `docs/SETUP.md` | 詳細セットアップガイド |
| `docs/README_ICONS.md` | アイコン仕様と生成方法 |
| `docs/QUICK_SETUP.md` | クイック実行ガイド |
| `docs/GITHUB_PUSH.md` | GitHub プッシュガイド |

## 🤝 開発・貢献

### ローカル開発

```bash
# リポジトリをクローン
git clone https://github.com/your-username/fishing-trip-memory.git
cd fishing-trip-memory

# Web サーバー起動
python -m http.server 8000

# http://localhost:8000 でアクセス
```

### コードスタイル
- Vanilla JavaScript（フレームワークなし）
- Tailwind CSS for UI
- コメント: 日本語

### 修正ルール
- コードの修正は必要箇所のみ
- 修正箇所以外の変更を禁止（コメント等含む）
- 修正指示は元のコードを正確に引用

## 📄 ライセンス

MIT License - 詳細は LICENSE ファイルを参照

## 👤 作成者

Created with ❤️ for fishing enthusiasts

## 🙏 謝辞

- [Tailwind CSS](https://tailwindcss.com/) - UI フレームワーク
- [Google Apps Script](https://script.google.com) - バックエンド
- [OpenWeatherMap](https://openweathermap.org/) - 天気API
- [JHA](https://www.jha.or.jp/) - 潮汐データ

---

**最終更新**: 2025年11月14日

**対応環境**:
- iOS 11.3+
- Android 5.0+
- デスクトップブラウザ (Chrome, Safari, Edge)

💡 **質問・バグ報告**: Issues タブからお願いします
