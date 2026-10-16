# Octotech Site behind Traefik edge

Bu quruluş OctoBot-u mövcud `edge` Traefik şəbəkəsi və `octobot-net` şəbəkəsi ilə birləşdirir. Traefik `agency2.octotech.az` hostunu HTTPS/443 üzərindən OctoBot-un daxili `8000` portuna yönləndirir. OctoBot portu hosta birbaşa açılmır.

## Tələblər

- Docker Engine və Docker Compose v2
- Mövcud `edge` və `octobot-net` Docker şəbəkələri
- `edge` şəbəkəsində `web` və `websecure` entrypoint-ləri olan Traefik
- `agency2.octotech.az` üçün DNS/Cloudflare Tunnel marşrutunun həmin Traefik edge-ə yönəlməsi

Şəbəkələr yoxdursa, yalnız onları yaradan hostda bu əmrlərlə yaradın:

```sh
docker network create edge
docker network create octobot-net
```

`edge` şəbəkəsini artıq Traefik yaradırsa, onu yenidən yaratmayın.

## Başlatma

Layihə qovluğunda:

```sh
docker compose -f compose.yaml -f compose.edge.yaml up -d
```

Vəziyyət və loglar:

```sh
docker compose -f compose.yaml -f compose.edge.yaml ps
docker compose -f compose.yaml -f compose.edge.yaml logs -f octotech-site
```

## Davamlı məlumat

Sazlamalar/profillər `octobot-user-data`, loglar `octobot-logs`, backtest faylları `octobot-backtesting`, tentacles `octobot-tentacles` adlı Docker həcmlərində qalır. Ehtiyat nüsxəni qorumaq üçün `docker compose down -v` işlətməyin.

## Təhlükəsizlik

OctoBot-un idarəetmə paneli üçün `agency2.octotech.az` ünvanında Cloudflare Access qaydası qurun və yalnız icazəli istifadəçilərə giriş verin. Birja API açarlarını yalnız ehtiyac varsa tətbiqin içindən daxil edin; çıxarış icazəsini aktiv etməyin.
