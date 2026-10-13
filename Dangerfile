import Danger

let danger = Danger()

// Check 1: PR Description
if let body = danger.github.pullRequest.body, body.isEmpty {
    warn("Please provide a PR description explaining what changed and why.")
} else if let body = danger.github.pullRequest.body, body.count < 20 {
    warn("The PR description is quite short. Please provide more context.")
}

// Check 2: PR Title
let title = danger.github.pullRequest.title

if title.isEmpty {
    fail("PR title must not be empty.")
} else if title.count < 10 {
    warn("PR title is too short. Please use a descriptive title.")
}

// Check 3: PR Labels
let labels = danger.github.pullRequest.labels.map { $0.name }

if labels.isEmpty {
    warn("Please add at least one label to categorise this PR.")
}

// Check 4: PR Size
let files = danger.git.createdFiles
    + danger.git.modifiedFiles
    + danger.git.deletedFiles

let changedFileCount = Set(files).count

if changedFileCount > 20 {
    warn("This PR changes \(changedFileCount) files. Consider splitting it into smaller PRs.")
}

// Check 5: Documentation
let changedFiles = Set(files)

let hasDocumentationChanges = changedFiles.contains {
    $0.lowercased().hasSuffix(".md")
    || $0.lowercased().contains("docs/")
}

if !hasDocumentationChanges {
    message("No documentation changes detected. Please consider whether documentation needs updating.")
}

// Confirmation
message("Danger checks completed. Please review any warnings or failures above.")
