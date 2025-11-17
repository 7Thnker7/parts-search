# CLAUDE.md - Krage Hardware Database

## Project Overview

This repository contains the **Krage Hardware Database**, a self-contained single-page web application for managing hardware inventory. The application is designed to track fasteners, hardware components, part numbers, and their locations.

**Repository**: 7Thnker7/parts-search
**Primary File**: `krage_hardware.rev2.html`
**Type**: Static HTML/CSS/JavaScript Application
**Version**: 2.0

## Project Purpose

The Krage Hardware Database is used to:
- Catalog hardware components (bolts, nuts, screws, washers, studs, rivets, etc.)
- Track multiple part numbers (Wurth PN, Trane PN)
- Manage inventory locations (FIN, SUB, WEL)
- Filter and search through hardware inventory
- Export data to CSV format
- Maintain revision history of database changes

## Codebase Structure

### File Organization
```
/
├── .git/                           # Git repository metadata
├── krage_hardware.rev2.html        # Main application (single file)
└── CLAUDE.md                       # This file
```

### Application Architecture

This is a **monolithic single-page application** where all components are embedded in one HTML file:

1. **HTML Structure** (lines 1-704)
   - Header with branding
   - Admin controls section
   - Search and filter controls
   - Data table
   - Modal dialogs for item management

2. **CSS Styling** (lines 7-535)
   - Responsive design with mobile breakpoints
   - Print-optimized styles
   - Modern gradient themes
   - Modal and form styling

3. **JavaScript Logic** (lines 733-1485)
   - Data management (CRUD operations)
   - Local storage persistence
   - Search/filter functionality
   - Admin mode with password protection
   - CSV export capability
   - Revision tracking system

## Key Features

### 1. Data Structure
Hardware items are stored as arrays with the following schema:
```javascript
[
  status,              // Index 0: "ACTIVE", "INACTIVE", or "OBSOLETE"
  itemId,             // Index 1: Unique identifier (e.g., "B0008")
  description,        // Index 2: Item description
  location,           // Index 3: Storage location (FIN/SUB/WEL)
  materialDesc,       // Index 4: Detailed material description
  itemIdDuplicate,    // Index 5: Duplicate ID for compatibility
  wurthPN,           // Index 6: Wurth part number
  tranePN            // Index 7: Trane part number
]
```

### 2. Admin Mode
- **Trigger**: Type `admin123` in the search box
- **Exit**: Press ESC or click "Exit Admin Mode"
- **Capabilities**: Add, edit, delete items; manage revisions

### 3. Local Storage
Data is persisted using browser localStorage with keys:
- `krageHardwareData` - Main inventory data
- `krageRevisionHistory` - Change tracking
- `krageCurrentRevision` - Version number
- `krageDataVersion` - Schema version (currently 2.0)

### 4. Filter System
Users can filter by:
- Search term (searches all columns)
- Status (ACTIVE/INACTIVE/OBSOLETE)
- Location (FIN/SUB/WEL/etc.)

## Development Workflows

### Making Changes to the Application

1. **Structural Changes**
   - Edit the HTML structure (lines 1-704)
   - Test responsive behavior at different breakpoints
   - Verify print styles work correctly

2. **Styling Updates**
   - Modify CSS in `<style>` block (lines 7-535)
   - Maintain consistency with existing color scheme:
     - Primary: `#667eea` (purple-blue)
     - Secondary: `#764ba2` (purple)
     - Success: `#28a745` (green)
     - Danger: `#dc3545` (red)
   - Test mobile responsiveness (breakpoints at 768px and 480px)

3. **Functionality Changes**
   - Edit JavaScript in `<script>` block (lines 733-1485)
   - Key functions to understand:
     - `initializeApp()` - Application initialization
     - `renderTable()` - Renders the data table
     - `filterTable()` - Applies search/filter logic
     - `saveToLocalStorage()` - Persists data
     - `enterAdminMode()` / `exitAdminMode()` - Admin controls

4. **Data Modifications**
   - Hardware data is initialized in `hardwareData` array (lines 735-823)
   - Changes through UI automatically save to localStorage
   - For bulk updates, consider export → edit JSON → import

### Testing Workflow

Since this is a client-side application:

1. **Open in Browser**
   ```bash
   # Simple HTTP server options:
   python3 -m http.server 8000
   # or
   npx serve .
   ```

2. **Test Checklist**
   - [ ] Search functionality works across all columns
   - [ ] Filters (Status, Location) work correctly
   - [ ] Admin mode can be entered and exited
   - [ ] CRUD operations persist to localStorage
   - [ ] CSV export generates valid files
   - [ ] Responsive design works on mobile
   - [ ] Print functionality produces clean output

3. **Browser Compatibility**
   - Primary target: Modern browsers (Chrome, Firefox, Safari, Edge)
   - Uses ES6+ features (arrow functions, template literals, spread operator)
   - localStorage API required

### Version Control

**Commit Guidelines:**
- Use descriptive commit messages
- Reference revision numbers when applicable
- Examples:
  - "Add new filter for manufacturer"
  - "Fix: Correct sort direction for Item ID column"
  - "Update: Rev 2.1 - Added batch delete feature"

**Branching Strategy:**
- Main branch: Production-ready code
- Feature branches: `claude/` prefix (as per current session)

## Key Conventions for AI Assistants

### 1. File Handling
- **NEVER** split this file into multiple files unless explicitly requested
- This is intentionally a single-file application for portability
- When making changes, use the Edit tool to modify specific sections

### 2. Data Integrity
- Always validate data structure before modifications
- Preserve the 8-element array structure for hardware items
- Test localStorage operations to ensure data persistence
- Never directly modify the `hardwareData` array initialization without backup

### 3. Admin Password
- Current password: `admin123` (line 835)
- If changing password, ensure it's communicated to the user
- Password is stored in plain text for simplicity (client-side only)

### 4. Styling Consistency
- Maintain the gradient theme (purple-blue color scheme)
- Keep button styles consistent with existing classes:
  - `btn-primary` - Main actions (purple)
  - `btn-success` - Create/save (green)
  - `btn-danger` - Delete/warning (red)
  - `btn-secondary` - Cancel/back (gray)
  - `btn-warning` - Admin actions (yellow)

### 5. Responsive Design
- Test changes at mobile breakpoints (768px, 480px)
- Use `.hide-mobile` class for non-essential columns on small screens
- Maintain touch-friendly button sizes (minimum 44x44px)

### 6. localStorage Versioning
- Current version: `2.0` (line 866, 908)
- When making breaking changes to data structure:
  1. Increment version number
  2. Update version check in `loadFromLocalStorage()`
  3. Document migration path in commit message

### 7. Code Modification Best Practices
- **Search before editing**: Use Grep to locate specific functions
- **Test incrementally**: Make small changes and test in browser
- **Preserve formatting**: Maintain 4-space indentation
- **Comment complex logic**: Add inline comments for non-obvious code
- **Backup data**: Remind users to export database before major changes

### 8. Common Tasks

**Adding a new column to the table:**
1. Update data structure documentation
2. Modify `hardwareData` initialization to include new field
3. Update `renderTable()` to display new column
4. Add header in HTML table structure (lines 623-632)
5. Update CSV export function (lines 1338-1357)
6. Increment localStorage version if breaking change

**Adding a new filter:**
1. Add filter UI in controls section (lines 575-616)
2. Implement filter logic in `filterTable()` (lines 1284-1306)
3. Add reset capability in `resetFilters()` (lines 1329-1336)
4. Update initialization in `initializeApp()` if needed

**Adding a new export format:**
1. Create export function (follow pattern of `exportToCSV()`)
2. Add button in controls section
3. Use `downloadFile()` utility (lines 1410-1421)

**Modifying admin capabilities:**
1. Check `isAdminMode` flag before sensitive operations
2. Update `enterAdminMode()` / `exitAdminMode()` as needed
3. Add UI controls in admin controls section (lines 552-572)

## Security Considerations

### Current Security Posture
- **Client-side only**: No backend server, no network requests
- **No authentication**: Admin password is purely UI-level protection
- **Local data**: All data stored in browser localStorage
- **No sensitive data**: Designed for hardware part numbers (public info)

### Limitations
- Admin password can be viewed in source code
- localStorage can be manipulated via browser dev tools
- No backup or sync mechanism
- No multi-user support

### Recommendations for Production Use
If this application needs to handle sensitive data or multi-user scenarios:
1. Implement backend API with proper authentication
2. Use server-side database (not localStorage)
3. Add role-based access control
4. Implement audit logging
5. Add data validation and sanitization
6. Consider adding automated backups

## Browser Compatibility

### Supported Browsers
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

### Required APIs
- `localStorage` - For data persistence
- `Blob` and `URL.createObjectURL()` - For file downloads
- ES6+ features (arrow functions, template literals, classes)

## Troubleshooting Guide

### Common Issues

**1. Data not persisting**
- Check browser localStorage quotas (typically 5-10MB)
- Verify localStorage is not disabled in browser settings
- Check browser console for errors

**2. Search not working**
- Verify search input event listeners are attached
- Check for JavaScript errors in console
- Ensure `filterTable()` is being called

**3. Admin mode not activating**
- Verify exact password match (case-sensitive)
- Check search input value in dev tools
- Ensure `enterAdminMode()` is defined

**4. Export fails**
- Check browser allows file downloads
- Verify `downloadFile()` function is working
- Test with smaller datasets first

**5. Styling issues**
- Clear browser cache
- Check for CSS conflicts
- Verify viewport meta tag is present

## Future Enhancement Ideas

Potential features for future development:
- [ ] Multiple user accounts with different access levels
- [ ] Backend API integration for multi-device sync
- [ ] Advanced search with regex support
- [ ] Batch operations (bulk edit, bulk delete)
- [ ] Image attachments for hardware items
- [ ] Barcode/QR code generation and scanning
- [ ] Integration with purchasing systems
- [ ] Inventory quantity tracking
- [ ] Low stock alerts
- [ ] Supplier contact information
- [ ] PDF export with formatted reports
- [ ] Dark mode toggle

## Resources

### Key Functions Reference
- `initializeApp()` - Initialize application state
- `renderTable()` - Render data table with current filters
- `filterTable()` - Apply search and filters
- `sortTable(column)` - Sort by column index
- `enterAdminMode()` / `exitAdminMode()` - Toggle admin mode
- `saveToLocalStorage()` / `loadFromLocalStorage()` - Data persistence
- `exportToCSV()` / `exportDatabase()` - Export functionality
- `showAddItemModal()` / `saveItem()` - Item CRUD operations

### Data Flow
```
User Action → Event Listener → Function Call → Data Modification →
saveToLocalStorage() → renderTable() → DOM Update
```

### localStorage Schema
```javascript
{
  "krageDataVersion": "2.0",
  "krageCurrentRevision": "1.0",
  "krageHardwareData": [[status, itemId, desc, ...], ...],
  "krageRevisionHistory": [{number, description, author, date}, ...]
}
```

## Contact & Support

For questions about this application:
- Check browser console for errors
- Review revision history in the application
- Export database for backup before making major changes

## Changelog

### Version 2.0 (Current)
- Updated data version to force refresh of localStorage
- Fixed table column ordering
- Improved responsive design
- Enhanced admin controls

### Version 1.0
- Initial database release
- Basic CRUD operations
- Search and filter functionality
- CSV export capability

---

**Last Updated**: 2025-11-17
**Maintained by**: AI Assistant (Claude)
**Original Author**: Ruben Valdez
