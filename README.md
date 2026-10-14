# renovate-config

Takudev の repo に共通の Renovate の設定（preset）の正本。各 repo の `renovate.json` から読み込む。

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>takudev-works/renovate-config"]
}
```

`github>takudev-works/renovate-config` は、この repo の main の `default.json` を指す。タグは打たず、main に入った変更がそのまま各 repo に効く。

## 共通にしているもの

| 設定 | 中身 |
| --- | --- |
| 実行の時間 | 平日の 9 時より前（日本時間）。lock ファイルの更新は月曜の 9 時より前 |
| PR | ラベル `dependencies`、レビュアーと担当は `t-i-0414`、コミットメッセージの頭に `chore:`、同時に開く PR は 5 本まで |
| 上げ方 | 公開から 7 日たった版だけを上げる。範囲指定（`^` など）の下限も上げる |
| まとめ方 | npm・Python・GitHub Actions・mise・Terraform のそれぞれで、major 以外を 1 本の PR にまとめる |
| major | Dependency Dashboard で承認してから PR を出す |
| TypeScript | 7 未満に止める。typescript-eslint が TS 7 にまだ対応していないため |
| `dev-config` | 自分たちで出すので、7 日待たず、major も承認を待たない |

repo ごとの事情（base のブランチ、ランタイムの制約、特定のパッケージの上限など）は、各 repo の `renovate.json` に書く。

## 変更の流れ

変更は PR で出し、`validate` が通ってから merge する。`validate` では、Markdown・スペル・秘密情報の検査と、`renovate-config-validator` による設定の検査をする。

手元では `mise run setup` で依存と lefthook を入れる。
