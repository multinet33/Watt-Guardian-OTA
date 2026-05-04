# Watt-Guardian-OTA

Manifest public pour la mise à jour automatique OTA pull du firmware [Watt-Guardian](https://github.com/multinet33/Watt-Guardian).

## Fichier `ota-manifest.json`

Lu par le firmware Guardian (FW348+) via :

```
https://raw.githubusercontent.com/multinet33/Watt-Guardian-OTA/main/ota-manifest.json
```

À chaque release :
1. Mettre à jour `build_fw` / `build_fs` / `version` / `released_at` / `changelog`
2. Pour chaque env supporté : URL GitHub Release du `firmware.bin`, SHA-256, taille
3. Commit + push sur `main` → device pollera et proposera la maj

## Format

Voir le fichier [ota-manifest.json](./ota-manifest.json) pour le format.

## Sécurité

- SHA-256 du `firmware.bin` est vérifié côté device avant flash
- HTTPS GitHub raw → certificat valide (skip cert verify côté device pour économie heap, intégrité par SHA)
- No-downgrade : refuse si `build_fw` manifest ≤ `build_fw` device

## Workflow

1. Builder le firmware localement (`pio run -e <env>`)
2. Uploader le `.bin` en GitHub Release sur le repo Watt-Guardian
3. Calculer `shasum -a 256 firmware.bin`
4. Mettre à jour ce manifest avec la nouvelle URL + SHA
5. `git commit && git push`

## Phase 4 EVO-104 (à venir)

Script Python `generate-manifest.py` qui automatise les étapes 3-4 ci-dessus,
intégré au workflow CI/CD GitHub Actions du repo Watt-Guardian.

## Licence

CC BY-NC-SA 4.0 (cohérent avec le firmware Watt-Guardian).
