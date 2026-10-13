# Check 1: PR Description
if github.pr_body.nil? || github.pr_body.strip.empty?
  warn("Please add a PR description.")
end

# Check 2: PR Title
title = github.pr_title

if title.nil? || title.strip.empty?
  fail("PR title must not be empty and should have at least 10 characters.")
elsif title.length < 10
  warn("PR title should have at least 10 characters.")
end

# Check 3: PR Labels
labels = github.pr_labels.map { |label| label.name }

if labels.empty?
  warn("Please add a label to this PR.")
end

# Check 4: PR Size
changed_files = (
  git.added_files +
  git.modified_files +
  git.deleted_files
).uniq

changed_file_count = changed_files.length

if changed_file_count > 20
  warn("This PR changes #{changed_file_count} files. Consider splitting it into smaller PRs.")
end

# Check 5: Documentation
has_documentation_changes = changed_files.any? do |file|
  file.downcase.end_with?(".md") ||
    file.downcase.start_with?("docs/")
end

unless has_documentation_changes
  message("No documentation changes detected. Please consider whether documentation needs updating.")
end

# Confirmation
message("Danger has completed the PR checks!")