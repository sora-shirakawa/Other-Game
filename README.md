# Neon Orbit Survivor (HTML Prototype)

スマートフォン向けの短時間リプレイ性を重視した 1 ボタン系回避ゲームです。

## 遊び方
- `スタート`で開始。
- 画面を左右にドラッグして自機を円軌道上で移動。
- 画面を押している間は**減速シールド**が発動（ゲージ消費）。
- 敵に当たると終了。敵をギリギリで避けるとコンボ倍率が上昇。

## リプレイ性の設計
- 時間経過で敵の生成間隔・速度が上がる「段階的難化」。
- ニアミス報酬（コンボ・スコア加速）で攻めるプレイを誘導。
- localStorage のベストスコア保存。
- 1 プレイ 30 秒〜2 分程度のサクサク設計。

## Unity(C#)移植しやすい構成
ゲームロジックを以下の責務に分割済みで、ほぼそのまま C# に移植可能です。

1. **GameState**
   - `score`, `combo`, `time`, `slowCharge`, `running`
2. **PlayerController**
   - `angle` と `radius` から `x,y` を算出
   - ドラッグ入力の差分で角度更新
3. **EnemySpawner**
   - 4 辺ランダム生成
   - 中心へ向かう速度ベクトル計算
4. **CollisionSystem**
   - プレイヤー当たり判定
   - ニアミス判定（1 回限りフラグ）
5. **UI/HUD**
   - score / best / combo / overlay

### Unity化の最短方針
- `Update()` で現在の `update(dt)` 相当処理。
- `FixedUpdate()` ではなく `Update()` + `Time.deltaTime` でも十分。
- `Player`, `Enemy`, `Pulse` を MonoBehaviour または ECS で管理。
- localStorage は `PlayerPrefs` に置換。

## 起動
`index.html` をブラウザで開くだけで遊べます。
