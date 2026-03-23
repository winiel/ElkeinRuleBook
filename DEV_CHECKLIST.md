# 🔍 Development Pre-Flight Checklist

**MANDATORY: Run before ANY development work!**

---

## ✅ Step 1: Check CodingGuide Updates

```bash
python3 /home/winielab/.openclaw/workspace/check_coding_guide_updates.py
```

### What it checks:
- ✓ `CodingGuide.md` - Project structure, file/function naming, SQL management
- ✓ `AwsServiceGuide.md` - AWS resource naming (EC2, RDS, S3, VPC, etc.)
- ✓ `DatabaseGuide.md` - Database/table/column naming, constraints, DDL standards

### If updates found:
1. 📖 Read the changed sections carefully
2. ⚠️ Apply to current work IMMEDIATELY
3. 🔧 Update existing code/infra to match new rules
4. 📝 Document changes in commit message

---

## ✅ Step 2: Review Applicable Guides

Based on your task:

### For Backend/API Development:
- Read: `CodingGuide.md`
- Read: `DatabaseGuide.md`

### For AWS Infrastructure:
- Read: `AwsServiceGuide.md`
- Read: `DatabaseGuide.md` (if RDS/database involved)

### For Full-Stack:
- Read: ALL three guides

---

## ✅ Step 3: Verify Environment

```bash
# Check current branch
git branch --show-current

# Check for uncommitted changes
git status

# Ensure dependencies up to date
# (npm install / pip install -r requirements.txt / etc.)
```

---

## 🚫 What NOT to Do

- ❌ Skip guide checks ("I'll do it later")
- ❌ Ignore update notifications
- ❌ Use old naming conventions
- ❌ Hardcode values that should be in .env
- ❌ Commit .env files

---

## 📚 Quick Reference

| Guide | Key Rules |
|-------|-----------|
| **CodingGuide** | • kebab-case files<br>• camelCase functions<br>• Raw SQL in `src/repositories/sql/` |
| **AwsServiceGuide** | • PascalCase resources<br>• Format: `[Project][Env][Service][Role]`<br>• S3 lowercase-hyphen |
| **DatabaseGuide** | • snake_case tables/columns<br>• Plural table names<br>• `created_at`/`updated_at` auto-update |

---

**Remember:** Guides change frequently. Always check before starting work! 🎯
