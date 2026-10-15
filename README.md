# Týmové skilly pro PHP/Nette vývoj

Sdílená sada workflow pravidel pro Codex a Claude Code. Každý skill je samostatný adresář s jediným obsahovým souborem `SKILL.md` ve formátu otevřeného standardu Agent Skills.

Instrukce skillů jsou anglicky kvůli přenositelnosti mezi různými agenty. Tento návod je česky pro týmovou správu a instalaci.

## Obsah

| Skill | Účel |
| --- | --- |
| `team-development-workflow` | Koordinuje celý implementační úkol od počáteční synchronizace po otestovaný commit a push. |
| `safe-git-delivery` | Provede jeden bezpečný pull na začátku a doručuje atomické, ověřené změny. |
| `project-risk-assessment` | Před implementací zmapuje projekt a konkrétní rizika. |
| `composer-dependency-maintenance` | Udržuje reprodukovatelné Composer závislosti a řídí samostatné aktualizace. |
| `nette-project-structure` | Určuje výchozí strukturu a hranice Nette aplikace. |
| `php-quality-gate` | Vyžaduje test změněného chování, testovací sadu, PHPStan a funkční ověření. |
| `relational-database-design` | Řídí návrh schématu, migrace, indexy, transakce a bezpečnost dat. |
| `php-84-runtime` | Udržuje minimální PHP 8.4 a kompatibilní novější verze PHP 8.x. |

## Výchozí workflow

Pro běžný implementační úkol použijte `team-development-workflow`. Ten propojuje ostatní pravidla v tomto pořadí:

1. načtení projektových instrukcí a kontrola pracovního stromu;
2. jednorázový `git pull --ff-only` na čisté větvi;
3. prohlídka synchronizovaného projektu a vyhodnocení rizik;
4. kontrola PHP 8.4+ a `composer install` podle existujícího lock souboru;
5. implementace v souladu s architekturou, databází a runtime politikou;
6. nový nebo upravený test pro změněné chování;
7. relevantní testy, celá povinná sada, PHPStan a funkční ověření;
8. atomický commit a okamžitý push aktuální větve.

Samotné `composer update` se před každým úkolem nespouští. Aktualizace závislostí je samostatná práce s vlastním diffem, validací, testy, PHPStanem a commitem.

## Instalace do projektu

Codex hledá projektové skilly v `.agents/skills/`, Claude Code v `.claude/skills/`. Oba nástroje umí načíst symlinkovaný adresář, takže obsah není nutné duplikovat.

Doporučený způsob je přidat tento repozitář do projektu jako submodule nebo subtree, například pod `.team-skills`, a z projektových adresářů vytvořit symlinky:

```sh
git submodule add <URL-TO-THIS-REPOSITORY> .team-skills
mkdir -p .agents/skills .claude/skills

for skill_dir in .team-skills/skills/*; do
    skill_name=$(basename "$skill_dir")
    ln -s "../../.team-skills/skills/$skill_name" ".agents/skills/$skill_name"
    ln -s "../../.team-skills/skills/$skill_name" ".claude/skills/$skill_name"
done
```

Před vytvořením symlinků ověřte, že cílové názvy už neexistují. Existující týmové nebo projektové skilly automaticky nepřepisujte.

Symlinky i odkaz na sdílený repozitář commitněte do projektu. Aktualizace týmových pravidel se pak provádí řízenou změnou verze submodulu nebo synchronizací subtree a projde běžným code review.

Pokud submodule ani subtree používat nechcete, lze jednotlivé adresáře pod `skills/` zkopírovat do obou umístění. V takovém případě ale vzniknou dvě kopie, které je nutné udržovat synchronizované.

## Osobní instalace

Pro použití ve všech lokálních projektech lze stejné adresáře symlinkovat do:

- Codex: `~/.agents/skills/<skill-name>/SKILL.md`
- Claude Code: `~/.claude/skills/<skill-name>/SKILL.md`

Projektová instalace je pro týmová pravidla vhodnější, protože přesnou verzi skillů sdílí celý repozitář a kontroluje code review.

## Použití

Agenti mohou skilly vybrat automaticky podle pole `description`. Koordinační skill lze vyvolat také explicitně:

- Codex: `$team-development-workflow`
- Claude Code: `/team-development-workflow`

Pro samostatný úkol lze vyvolat přímo například `$project-risk-assessment` v Codexu nebo `/project-risk-assessment` v Claude Code.

Automatický commit a push platí pro implementační práci, kterou má agent oprávnění provést. Skilly nedávají oprávnění zahodit cizí změny, přepsat historii, obejít ochranu větve, pracovat s produkčními daty nebo rozšířit zadání. Read-only analýza, plán či review se necommitují.

## Údržba

- Jeden skill řeší jednu jasnou odpovědnost; společné pořadí drží koordinační skill.
- Změny pravidel kontrolujte stejně jako změny kódu.
- Pole `name` musí odpovídat názvu adresáře a zůstat unikátní.
- Pole `description` musí jasně říkat, kdy se má skill použít a kdy nikoliv.
- Nevkládejte do skillů projektová hesla, tokeny, interní produkční adresy ani jiné tajné údaje.
- Před vydáním nové verze ověřte YAML frontmatter všech skillů a otestujte alespoň modelový implementační, databázový a dependency scénář.

## Primární dokumentace

- [OpenAI: Build skills](https://developers.openai.com/codex/skills/)
- [Claude Code: Extend Claude with skills](https://code.claude.com/docs/en/skills)
- [Nette: adresářová struktura aplikace](https://doc.nette.org/cs/application/directory-structure)
- [Composer: základní použití install a update](https://getcomposer.org/doc/01-basic-usage.md)
- [PHPStan: Getting Started](https://phpstan.org/user-guide/getting-started)

