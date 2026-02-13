# Prompt Templates for Common Task Types

## Template 1: Add Delete Button (CRUD Operation)

```markdown
## Task: Add Delete [ENTITY] Button
**Group:** PARALLEL Group [N]
**Dependencies:** None
**Output File:** `.task-outputs/delete-[entity].md`

### Pre-Flight Check
No dependencies - can start immediately.

### Files to Modify
- `/src/components/admin/[entities]/[Entity]Profile.tsx` - Add delete button UI
- `/src/lib/actions/[entities].ts` - Add delete[Entity] server action

### Requirements
1. Add a "Delete [Entity]" button in the [Entity]Profile component header
2. Style as destructive action (rose/red color, pill button rounded-[50px])
3. Show confirmation dialog using existing ConfirmDialog component
4. Create server action that:
   - Checks admin access
   - [If has children] Handles cascade delete or blocks with error
   - Deletes the record
   - Returns success/error
5. Redirect to /admin/[entities] after successful deletion
6. Handle errors with user feedback

### Implementation Steps
1. Read the existing [Entity]Profile.tsx to understand structure
2. Import ConfirmDialog from `/src/components/admin/shared/ConfirmDialog.tsx`
3. Add state for dialog visibility: `const [showDeleteDialog, setShowDeleteDialog] = useState(false)`
4. Add delete button next to existing Edit button
5. Add ConfirmDialog with warning message
6. Create delete[Entity] server action in [entities].ts following existing patterns
7. Call server action on confirm, handle result

### Validation
- [ ] Delete button appears in profile header
- [ ] Confirmation dialog shows on click
- [ ] Server action validates admin access
- [ ] Successful deletion redirects to list
- [ ] Errors show user-friendly message
- [ ] Build passes: `npm run build`

### Output
When complete, create `.task-outputs/delete-[entity].md` with:
```
# Task: Delete [Entity] Button
STATUS: COMPLETE
FILES_MODIFIED:
- /src/components/admin/[entities]/[Entity]Profile.tsx
- /src/lib/actions/[entities].ts
NOTES:
- Added delete[Entity] server action
- Uses ConfirmDialog for confirmation
```
```

---

## Template 2: Database Migration

```markdown
## Task: Database Migration - [DESCRIPTION]
**Group:** SEQUENTIAL Group [N] - MUST DO FIRST
**Dependencies:** None (but blocks other tasks)
**Output File:** `.task-outputs/migration-[name].md`

### Pre-Flight Check
No dependencies - but this task BLOCKS dependent tasks.

### Migration Details
- **Table(s):** [table_name]
- **Change Type:** [ALTER/CREATE/ADD COLUMN]
- **Columns:** [column details]

### Requirements
1. Create migration that [specific changes]
2. Preserve existing data during migration
3. Set appropriate defaults for new columns
4. Create RLS policies if new table
5. Update TypeScript types after migration

### Implementation Steps
1. Use Supabase MCP `apply_migration` tool:
   ```sql
   -- Migration: [name]
   [SQL HERE]
   ```
2. Verify migration applied: `mcp__supabase__list_migrations`
3. Generate updated types: `mcp__supabase__generate_typescript_types`
4. Update `/src/types/database.ts` if manually maintained

### Validation
- [ ] Migration applied successfully
- [ ] Existing data preserved
- [ ] New columns/tables have correct types
- [ ] RLS policies in place (if applicable)
- [ ] TypeScript types updated

### Output
When complete, create `.task-outputs/migration-[name].md` with:
```
# Task: Database Migration - [DESCRIPTION]
STATUS: COMPLETE
MIGRATION_NAME: [timestamp]_[name]
TABLES_AFFECTED:
- [table1]
- [table2]
NOTES:
- [Any important notes about data migration]
- Dependent tasks can now proceed
```
```

---

## Template 3: UI Component Update

```markdown
## Task: Update [COMPONENT] UI
**Group:** PARALLEL Group [N]
**Dependencies:** [None / Requires migration X]
**Output File:** `.task-outputs/ui-[component].md`

### Pre-Flight Check
[If has dependencies]
Before starting:
1. Check `.task-outputs/[dependency].md` exists
2. Confirm STATUS: COMPLETE
3. If not, STOP and wait

### Files to Modify
- `/src/components/[path]/[Component].tsx` - [Changes]
- `/src/app/[path]/page.tsx` - [Changes if needed]

### Design Requirements
Follow "Lavender Keepsake" design system:
- Colors: sage-400 (primary), rose-400 (accent), oatmeal/cream (backgrounds)
- Typography: Solway (headers), Nunito (body)
- Buttons: rounded-[50px] pill shape
- Cards: rounded-xl

### Requirements
1. [Specific UI change 1]
2. [Specific UI change 2]
3. Maintain responsive design (mobile/tablet/desktop)
4. Keep existing functionality intact

### Implementation Steps
1. Read current component to understand structure
2. [Specific step 2]
3. [Specific step 3]
4. Test responsive behavior

### Validation
- [ ] UI changes visible and correct
- [ ] Responsive on mobile/tablet/desktop
- [ ] Existing functionality not broken
- [ ] Matches design system
- [ ] Build passes: `npm run build`

### Output
When complete, create `.task-outputs/ui-[component].md` with:
```
# Task: Update [COMPONENT] UI
STATUS: COMPLETE
FILES_MODIFIED:
- [list files]
NOTES:
- [Any notes]
```
```

---

## Template 4: Form Field Changes

```markdown
## Task: Update [FORM] Form Fields
**Group:** [PARALLEL/SEQUENTIAL] Group [N]
**Dependencies:** [None / Requires migration X]
**Output File:** `.task-outputs/form-[name].md`

### Pre-Flight Check
[Include if has DB dependency]

### Files to Modify
- `/src/components/[path]/[Form]Modal.tsx` - Form UI changes
- `/src/lib/actions/[entity].ts` - Server action input type updates

### Requirements
1. [Add/Remove/Modify] field: [field_name]
2. Update validation: [validation rules]
3. Update server action input type
4. [Other requirements]

### Implementation Steps
1. Read current form component
2. [Add/remove] form field for [field_name]
3. Update form state handling
4. Update validation schema
5. Update server action to handle new field structure
6. Test form submission

### Validation
- [ ] Form displays correctly
- [ ] Validation works as expected
- [ ] Server action accepts new input
- [ ] Data saves correctly to database
- [ ] Build passes: `npm run build`

### Output
When complete, create `.task-outputs/form-[name].md` with:
```
# Task: Update [FORM] Form
STATUS: COMPLETE
FILES_MODIFIED:
- [list files]
FIELD_CHANGES:
- Added: [fields]
- Removed: [fields]
- Modified: [fields]
```
```

---

## Template 5: Server Action (New)

```markdown
## Task: Create [ACTION_NAME] Server Action
**Group:** PARALLEL Group [N]
**Dependencies:** [None / Requires migration X]
**Output File:** `.task-outputs/action-[name].md`

### Pre-Flight Check
[Include if has DB dependency]

### Files to Modify
- `/src/lib/actions/[entity].ts` - Add new server action

### Requirements
1. Function signature: `[actionName](input: InputType): Promise<ActionResult>`
2. Access control: [Admin only / User's own data]
3. Database operations: [SELECT/INSERT/UPDATE/DELETE]
4. Return type: Follow existing ActionResult pattern

### Implementation Steps
1. Read existing actions in the file for patterns
2. Define input type (if new)
3. Implement action with:
   - Access control check
   - Input validation
   - Database operation
   - Error handling
   - Cache revalidation if needed
4. Export from module

### Pattern Example
```typescript
export async function [actionName](input: InputType): Promise<ActionResult> {
  const supabase = await createClient()

  // Access check
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) return { success: false, error: 'Unauthorized' }

  // [Admin check if needed]

  // Database operation
  const { data, error } = await supabase
    .from('[table]')
    .[operation]([params])

  if (error) return { success: false, error: error.message }

  revalidatePath('[path]')
  return { success: true, data }
}
```

### Validation
- [ ] Action exported correctly
- [ ] Access control working
- [ ] Database operation successful
- [ ] Error handling complete
- [ ] Build passes: `npm run build`

### Output
When complete, create `.task-outputs/action-[name].md` with:
```
# Task: Create [ACTION_NAME] Server Action
STATUS: COMPLETE
FILES_MODIFIED:
- /src/lib/actions/[entity].ts
ACTION_SIGNATURE:
- [actionName](input): Promise<ActionResult>
```
```

---

## Template 6: Complex Feature (Multi-File)

```markdown
## Task: Implement [FEATURE_NAME]
**Group:** SEQUENTIAL Group [N] - Task [X]
**Dependencies:** Requires Task [X-1] to complete
**Output File:** `.task-outputs/feature-[name].md`

### Pre-Flight Check
1. Check `.task-outputs/[dependency-1].md` - STATUS: COMPLETE
2. Check `.task-outputs/[dependency-2].md` - STATUS: COMPLETE
3. If any incomplete, STOP and report

### Overview
[Brief description of the feature]

### Files to Modify
- `/src/components/[path1]` - [Purpose]
- `/src/components/[path2]` - [Purpose]
- `/src/lib/actions/[file].ts` - [Purpose]
- `/src/app/[path]/page.tsx` - [Purpose]

### Requirements
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

### Implementation Steps

#### Step 1: [Component/Action Name]
1. [Detail]
2. [Detail]

#### Step 2: [Component/Action Name]
1. [Detail]
2. [Detail]

#### Step 3: Integration
1. [Detail]
2. [Detail]

### Validation
- [ ] [Feature-specific check 1]
- [ ] [Feature-specific check 2]
- [ ] All components render without error
- [ ] User flow works end-to-end
- [ ] Build passes: `npm run build`
- [ ] No lint errors: `npm run lint`

### Output
When complete, create `.task-outputs/feature-[name].md` with:
```
# Task: Implement [FEATURE_NAME]
STATUS: COMPLETE
FILES_MODIFIED:
- [list all files]
COMPONENTS_ADDED:
- [list new components]
ACTIONS_ADDED:
- [list new actions]
NOTES:
- [Integration notes]
- [Any caveats or follow-up items]
```
```

---

## Dependency Check Script

For tasks with dependencies, include this check pattern:

```typescript
// At the start of your work, verify dependencies
// Read the dependency output files and check STATUS

// Example check (conceptual - agent should read files):
const dependency1 = await readFile('.task-outputs/migration-xyz.md')
if (!dependency1.includes('STATUS: COMPLETE')) {
  console.log('BLOCKED: Dependency migration-xyz not complete')
  return // Stop work
}
```

---

## Status Markers

Use these consistent status markers:

| Status | Meaning |
|--------|---------|
| `STATUS: COMPLETE` | Task finished successfully |
| `STATUS: BLOCKED` | Waiting on dependency |
| `STATUS: FAILED` | Task failed, see notes |
| `STATUS: IN_PROGRESS` | Currently being worked on |
