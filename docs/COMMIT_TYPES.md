# 📝 Tipos de Commits (Conventional Commits)

Usa estos prefijos al hacer commits para mantener un historial claro:

| Tipo | Uso | Ejemplo |
|------|-----|---------|
| `feat:` | Nueva característica | `feat: agregar autenticación` |
| `fix:` | Bug fix | `fix: resolver error de login` |
| `docs:` | Documentación | `docs: actualizar README` |
| `style:` | Formato/espacios | `style: formatear código` |
| `refactor:` | Reorganización | `refactor: simplificar función` |
| `test:` | Tests | `test: agregar tests de auth` |
| `chore:` | Tareas admin | `chore: actualizar npm` |
| `ci:` | CI/CD | `ci: agregar GitHub Actions` |

## ✅ Beneficios

- Historial de cambios **legible**
- Cambios **trazables**
- Facilita **búsquedas**
- Profesional ante otros developers

## 🚫 Lo que NO debes hacer

```bash
# ❌ MALO
git commit -m "cambios"
git commit -m "arreglé esto"
git commit -m "falta una coma"

# ✓ BIEN
git commit -m "fix: resolver error en validación"
git commit -m "docs: mejorar instrucciones de setup"
git commit -m "style: agregar point and comma"