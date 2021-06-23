# 17. Activity log entries description.

Date: 2021-06-23

## Status

In progress

## Context

Description of activity log entries.

## Entries description

UserID | ActionType | DoerID | RootID | ObjectID | ParentID
--- | --- | --- | --- | --- | ---
userID | ActionTypeNewPoiReview [1] | authorID | - | reviewID | -
userID | ActionTypeNewKitchenReview [2] | authorID | - | reviewID | -
userID | ActionTypeNewFollower [3] | followerID | - | userID | -
userID | ActionTypeNewFollowRequest [4] | followerID | - | userID | -
userID | ActionTypeMyFollowRequestApproved [5] | followerID | - | userID | -
authorID | ActionTypeNewLike [6] | userID | - | reviewID | -
authorID | ActionTypeNewComment [7] | userID | reviewID | commentID | -
userID | ActionTypeUserTagInReview [8] | authorID | - | reviewID | -
userID | ActionTypeUserTagInComment [9] | authorID | reviewID | commentID | -
